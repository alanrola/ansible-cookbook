# root-command-log

Log every interactive shell command to syslog (`local6.notice`) and store it in `/var/log/rootcmd.log` on **EL 8/9** (RHEL/Rocky/Alma).

> Zombies can’t be killed—oh wait, wrong role 😉  
> Reminder here: this role **does not change** user privileges; it only logs commands.

---

## 🧩 What it does

- Inserts a `PROMPT_COMMAND` block into users’ `~/.bashrc` to log each command via `logger -p local6.notice`.
- Inserts environment initialization into `~/.bash_profile` (variables like `HOSTNAME`, `IP`, `USUARIO`).
- Ensures the same blocks exist in `/etc/skel` so new users inherit them.
- Adds an **rsyslog** rule to write `local6.notice` to `/var/log/rootcmd.log` and restarts `rsyslog`.

Compatible with **EL 8/9** (gated inside the tasks).

---

## 🔧 Variables

This role requires **no variables**.

---

## 🚀 Usage

Minimal play:
```yaml
- hosts: all
  become: true
  roles:
    - role: root-command-log
```

---

## ✅ Verify

```bash
# Check rsyslog rule is active
grep -F 'local6.notice' /etc/rsyslog.conf

# Send a test message
logger -p local6.notice "root-command-log test message"

# Tail the log file
tail -n 20 /var/log/rootcmd.log
```

Open a new shell session as a user and run a couple of commands; they should appear in `/var/log/rootcmd.log`.

---

## 🛠 How it works

- **Bash hooks:** writes an `ANSIBLE MANAGED BLOCK` to `~/.bashrc` with a `PROMPT_COMMAND` that appends history and emits to syslog.  
- **Environment:** adds an `ANSIBLE MANAGED BLOCK` to `~/.bash_profile` to set variables used in the log tag (e.g., `USUARIO`, `IP`).  
- **rsyslog:** appends a `local6.notice` rule to `/etc/rsyslog.conf` and restarts the service.

> The role targets all regular users (UID ≥ 1000) plus **root**, and mirrors the blocks in `/etc/skel`.

---

## ⚠️ Notes

- Command lines may contain **sensitive arguments**; review compliance requirements before enabling in production.
- The logging only affects **interactive bash** sessions that read `~/.bashrc`/`~/.bash_profile`.
- Keep in mind that non-bash shells (e.g., zsh, fish) are out of scope.

---

## 🗑️ Rollback

To remove:
1. Delete the **ANSIBLE MANAGED BLOCK** sections from users’ `~/.bashrc` and `~/.bash_profile`, and from `/etc/skel/`.
2. Remove the `local6.notice ... /var/log/rootcmd.log` line from `/etc/rsyslog.conf`.
3. Restart rsyslog: `systemctl restart rsyslog`.
