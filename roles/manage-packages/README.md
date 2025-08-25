# manage-packages

Role to **install/upgrade** and **remove** packages on RHEL-like systems, with optional **cache update** and **version pinning**.

> This role is simple, readable, and idempotent. Review and adapt before using in production.

---

## ✅ What it does

- Optionally **updates DNF cache**.
- **Installs / upgrades** a list of packages.
- **Removes** a list of packages.
- Optionally **pins exact versions** (installs `name-version`).

---

## 🧩 Requirements

- Target OS: **RHEL / Rocky / Alma (EL 8/9)**  
  > Cache step uses `dnf`. For EL7, replace with `yum` or drop it.
- Privileges: typically `become: true`.

---

## 🔧 Role variables

All defaults live in `defaults/main.yml`.

<!-- Role variables -->
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Type</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>update_cache</code></td>
      <td><code>bool</code></td>
      <td><code>false</code></td>
      <td>If <code>true</code>, refresh DNF cache before changes.</td>
    </tr>
    <tr>
      <td><code>packages_to_install</code></td>
      <td><code>list[string]</code></td>
      <td><code>[]</code></td>
      <td>Packages ensured <strong>present</strong> (install/upgrade).</td>
    </tr>
    <tr>
      <td><code>packages_to_remove</code></td>
      <td><code>list[string]</code></td>
      <td><code>[]</code></td>
      <td>Packages ensured <strong>absent</strong> (remove).</td>
    </tr>
    <tr>
      <td><code>packages_pinned</code></td>
      <td><code>list[dict]</code></td>
      <td><code>[]</code></td>
      <td>Exact versions to install; items like <code>{ name: "pkg", version: "X.Y.Z" }</code>.</td>
    </tr>
  </tbody>
</table>

Example for `packages_pinned`:
```yaml
packages_pinned:
  - { name: "nano", version: "2.9.8-1.el8" }
  - { name: "curl", version: "7.76.1-23.el9" }
```

> Pinning installs the specified version **now**; it does **not** block future upgrades.  
> For long-term holds, consider `dnf versionlock` (out of scope for this role).

---

## 🚀 Usage

### 1) Minimal playbook
```yaml
- name: Manage packages
  hosts: all
  become: true
  roles:
    - role: manage-packages
      vars:
        packages_to_install:
          - vim
          - git
```

### 2) Install and remove in the same run
```yaml
- name: Install and remove packages
  hosts: all
  become: true
  roles:
    - role: manage-packages
      vars:
        update_cache: true
        packages_to_install: [htop, wget]
        packages_to_remove: [telnet]
```

### 3) Pin exact versions
```yaml
- name: Pin exact versions
  hosts: all
  become: true
  roles:
    - role: manage-packages
      vars:
        packages_pinned:
          - { name: "nano", version: "2.9.8-1.el8" }
          - { name: "curl", version: "7.76.1-23.el9" }
```

### 4) Quick one-off via CLI
```bash
ansible-playbook -i inventory/hosts.ini site.yml \
  -e '{update_cache: true, packages_to_install: ["vim","git"]}'
```

---

## 🧪 Idempotency

- `package: state=present/absent` keeps runs idempotent.  
- Cache refresh may report `changed` when metadata updates.

---

## 🧾 Defaults (reference)
```yaml
# defaults/main.yml
update_cache: false
packages_to_install: []
packages_to_remove: []
packages_pinned: []
```

## 🔍 Task summary (reference)
```yaml
- name: Update DNF cache (optional)
  dnf:
    update_cache: true
  when: (update_cache | default(false)) | bool

- name: Install or upgrade packages
  package:
    name: "{{ packages_to_install }}"
    state: present
  when: (packages_to_install | default([])) | length > 0

- name: Remove packages
  package:
    name: "{{ packages_to_remove }}"
    state: absent
  when: (packages_to_remove | default([])) | length > 0

- name: Ensure pinned package versions (optional)
  package:
    name: "{{ item.name }}-{{ item.version }}"
    state: present
  loop: "{{ packages_pinned | default([]) }}"
  when: (packages_pinned | default([])) | length > 0
```

---
