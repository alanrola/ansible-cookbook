# disable-account

Disable a local account on **EL 8/9**: lock password, expire account, set shell to `nologin`, remove sudoers drop-in, revoke SSH keys (with backup), delete user crontab, and optionally kill processes.

> Safe-by-default but decisive. Review variables before running in production.

---

## 🔧 Variables

<table>
  <thead>
    <tr>
      <th>Variable</th><th>Default</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>user_name</code></td><td><em>required</em></td><td>Account to disable.</td></tr>
    <tr><td><code>disable_lock_password</code></td><td><code>true</code></td><td>Run <code>usermod -L</code> via Ansible <code>user</code> module.</td></tr>
    <tr><td><code>disable_expire_account</code></td><td><code>true</code></td><td>Expire with <code>chage -E 0</code>.</td></tr>
    <tr><td><code>disable_shell_nologin</code></td><td><code>true</code></td><td>Set shell to first available: <code>/sbin/nologin</code>, <code>/usr/sbin/nologin</code>, or <code>/bin/false</code>.</td></tr>
    <tr><td><code>disable_remove_sudoers</code></td><td><code>true</code></td><td>Delete <code>/etc/sudoers.d/&lt;user&gt;</code> if present.</td></tr>
    <tr><td><code>disable_revoke_keys</code></td><td><code>true</code></td><td>Remove <code>~/.ssh/authorized_keys</code>.</td></tr>
    <tr><td><code>disable_backup_keys</code></td><td><code>true</code></td><td>Backup authorized_keys to <code>{{ disable_backup_dir }}/&lt;user&gt;/authorized_keys.&lt;timestamp&gt;</code> before removal.</td></tr>
    <tr><td><code>disable_remove_crontab</code></td><td><code>true</code></td><td>Delete the user's crontab (<code>crontab -r -u &lt;user&gt;</code>).</td></tr>
    <tr><td><code>disable_kill_processes</code></td><td><code>true</code></td><td>Send <code>SIGKILL</code> to all processes by that user (<code>pkill -KILL -u &lt;user&gt;</code>).</td></tr>
    <tr><td><code>disable_backup_dir</code></td><td><code>/root/disabled-accounts</code></td><td>Base directory to store backups of authorized_keys.</td></tr>
  </tbody>
</table>

---

## 🚀 Usage

Minimal:
```yaml
- hosts: all
  become: true
  roles:
    - role: disable-account
      vars:
        user_name: olduser
```

Conservative (no KILL, keep crontab, keep keys):
```yaml
- hosts: all
  become: true
  roles:
    - role: disable-account
      vars:
        user_name: contractor
        disable_kill_processes: false
        disable_remove_crontab: false
        disable_revoke_keys: false
```

---

## ✅ Verify

```bash
getent passwd olduser          # check shell is nologin/false
passwd -S olduser              # should be locked
sudo -l -U olduser             # no sudoers entry
crontab -l -u olduser          # should fail (no crontab)
pgrep -u olduser || echo ok    # no processes
test -f /root/disabled-accounts/olduser/authorized_keys.* && echo "backup exists"
```

---

## ⚠️ Notes

- If the user logs in with non-bash shells, SSH key removal is the most effective block.
- You can re-enable the account by reversing the steps (set a valid shell, unlock password, restore keys if needed).
- The role is a no-op if the user does not exist.

---

## 📜 License
Apache-2.0
