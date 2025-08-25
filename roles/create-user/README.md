# create-user

Create (or remove) a local user on **EL 8/9** with optional groups, sudo privileges, password/lock, and SSH authorized keys.

> Simple and idempotent. Review variables before using in production.

---

## 🔧 Variables

<table>
  <thead>
    <tr>
      <th>Variable</th><th>Default</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>user_state</code></td><td><code>present</code></td><td>Set to <code>absent</code> to delete the account.</td></tr>
    <tr><td><code>user_remove_home</code></td><td><code>false</code></td><td>When deleting, remove home directory as well.</td></tr>
    <tr><td><code>user_name</code></td><td><em>required</em></td><td>Account name to manage.</td></tr>
    <tr><td><code>user_uid</code></td><td><code>~</code></td><td>Optional UID (integer).</td></tr>
    <tr><td><code>user_shell</code></td><td><code>/bin/bash</code></td><td>Login shell.</td></tr>
    <tr><td><code>user_home</code></td><td><code>~</code></td><td>Home path (defaults to <code>/home/&lt;user&gt;</code>).</td></tr>
    <tr><td><code>user_create_home</code></td><td><code>true</code></td><td>Create home if missing.</td></tr>
    <tr><td><code>user_password</code></td><td><code>~</code></td><td>Hashed password (e.g. SHA-512). Omit to leave unchanged.</td></tr>
    <tr><td><code>user_password_lock</code></td><td><code>false</code></td><td>Lock account without altering password hash.</td></tr>
    <tr><td><code>user_groups</code></td><td><code>[]</code></td><td>Supplemental groups (ensured present; appended).</td></tr>
    <tr><td><code>user_append_groups</code></td><td><code>true</code></td><td>Append instead of replace group membership.</td></tr>
    <tr><td><code>user_sudo</code></td><td><code>false</code></td><td>Create a <code>/etc/sudoers.d/&lt;user&gt;</code> entry.</td></tr>
    <tr><td><code>user_sudo_nopasswd</code></td><td><code>false</code></td><td>Use <code>NOPASSWD: ALL</code> if true.</td></tr>
    <tr><td><code>user_ssh_authorized_keys</code></td><td><code>[]</code></td><td>List of public keys to install at <code>~/.ssh/authorized_keys</code>.</td></tr>
  </tbody>
</table>

---

## 🚀 Usage

Minimal:
```yaml
- hosts: all
  become: true
  roles:
    - role: create-user
      vars:
        user_name: deploy
```

With groups and sudo (passwordless):
```yaml
- hosts: all
  become: true
  roles:
    - role: create-user
      vars:
        user_name: deploy
        user_groups: [wheel, docker]
        user_sudo: true
        user_sudo_nopasswd: true
```

With SSH keys and hashed password:
```yaml
- hosts: all
  become: true
  roles:
    - role: create-user
      vars:
        user_name: app
        user_password: "$6$rounds=656000$...$...hashed..."  # sha-512
        user_ssh_authorized_keys:
          - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... user@host"
          - "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ... user2@host"
```

Remove user and home:
```yaml
- hosts: all
  become: true
  roles:
    - role: create-user
      vars:
        user_name: olduser
        user_state: absent
        user_remove_home: true
```

---

## ✅ Verify

```bash
id deploy
getent passwd app
getent group wheel | grep deploy
sudo -l -U deploy
ls -ld /home/app/.ssh && ls -l /home/app/.ssh/authorized_keys
```
