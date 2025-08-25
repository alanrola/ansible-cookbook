# kill-processes-zombie

Finds **zombie processes** (state `Z`) and attempts cleanup by signalling their **parent processes (PPIDs)**.

> Reminder: zombies cannot be killed directly; only their **parent** can reap them.  
> The role starts in **dry-run** mode for safety.

---

## 🔧 Variables

<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>dry_run</code></td><td><code>true</code></td><td>Show intended actions without sending signals. Set to <code>false</code> to act.</td></tr>
    <tr><td><code>ppid_exclude</code></td><td><code>[1]</code></td><td>PPID list to never signal (e.g., <code>1</code> for <code>systemd/init</code>).</td></tr>
    <tr><td><code>parent_name_exclude</code></td><td><code>[ "systemd", "init" ]</code></td><td>Skip parents whose command matches any of these names.</td></tr>
    <tr><td><code>kill_signal_term</code></td><td><code>"TERM"</code></td><td>First signal sent to parent PIDs.</td></tr>
    <tr><td><code>kill_signal_kill</code></td><td><code>"KILL"</code></td><td>Fallback signal if zombies persist after <code>TERM</code>.</td></tr>
    <tr><td><code>wait_after_term</code></td><td><code>3</code></td><td>Seconds to wait after <code>SIGTERM</code> before checking again.</td></tr>
  </tbody>
</table>

---

## 🚀 Usage

Dry-run (default, safe):
```yaml
- hosts: all
  become: true
  roles:
    - role: kill-processes-zombie
```

Actually signal parents:
```yaml
- hosts: all
  become: true
  roles:
    - role: kill-processes-zombie
      vars:
        dry_run: false
```

Exclude specific parents:
```yaml
- hosts: all
  become: true
  roles:
    - role: kill-processes-zombie
      vars:
        dry_run: false
        ppid_exclude: [1, 1234, 5678]
        parent_name_exclude: [systemd, init, sshd]
```

---

## 🛠 What the role does (summary)

1. Lists processes and identifies zombies (`STAT` contains `Z`).  
2. Gathers their **parent PIDs** (PPIDs) and filters by:
   - `ppid_exclude` (numbers)
   - `parent_name_exclude` (command names)
3. **Dry-run**: prints candidates without signalling (default).  
4. When `dry_run: false`:
   - Sends **SIGTERM** to parent PIDs, waits `wait_after_term` seconds.
   - If zombies still exist under a PPID, sends **SIGKILL** to that parent.

---

## ⚠️ Notes

- Signalling a parent can affect **non-zombie children** from the same parent. Use with care.  
- Keep `ppid_exclude: [1]` to avoid disrupting **PID 1**.  
- Tested on EL 8/9; adjust commands if your platform differs.
