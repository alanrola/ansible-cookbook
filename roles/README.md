# 🧩 Roles

Reusable **Ansible roles** that group tasks, handlers, templates and files around a single, clear purpose.

> **Note:** Roles here are intended as **learning-friendly, reusable “recipes”**.  
> Review and adapt before using in production.

---

## 📖 Available Roles

<!-- Keep this table short and simple. Add one row per role you publish. -->
<table>
  <thead>
    <tr>
      <th>🧩 Role</th>
      <th>📝 Description</th>
      <th>▶️ Minimal usage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>manage-packages</code></td>
      <td>Installs/upgrades or removes packages, optionally updates DNF cache and pins specific versions.</td>
      <td><code>- role: manage-packages</code> with <code>packages_to_install: [vim, git]</code></td>
    </tr>
    <tr>
      <td><code>install_node_exporter</code></td>
      <td>Installs Prometheus node_exporter and manages it as a systemd service (EL 8/9).</td>
      <td><code>- role: install_node_exporter</code></td>
    </tr>
    <tr>
      <td><code>kill-processes-zombie</code></td>
      <td>Finds zombie processes and signals their parent PIDs to clean them up; starts in dry-run for safety.</td>
      <td><code>- role: kill-processes-zombie</code> with <code>dry_run: false</code></td>
    </tr>
    <tr>
      <td><em>(coming soon)</em></td>
      <td>First role will appear here.</td>
      <td>—</td>
    </tr>
  </tbody>
</table>

---

## 🛠️ How to use a role in a playbook

```yaml
- name: Apply a role example
  hosts: all
  become: true
  roles:
    - role: my_role_name
```

## 📦 Standard role layout

Recommended **Galaxy-style** structure for a role:

```
roles/
└── my_role_name/
    ├── defaults/
    │   └── main.yml          # Low-priority defaults (document your vars here)
    ├── files/                # Static files (copied as-is)
    ├── handlers/
    │   └── main.yml          # Handlers (restarts, notifications)
    ├── meta/
    │   └── main.yml          # galaxy_info, dependencies, supported platforms
    ├── tasks/
    │   └── main.yml          # Role entry point
    ├── templates/            # Jinja2 templates (*.j2)
    ├── vars/
    │   └── main.yml          # High-priority vars (use sparingly)
    ├── README.md             # Role documentation (what it does, vars, examples)
    └── tests/                # (Optional) Simple tests: playbook + inventory
        ├── inventory
        └── test.yml
```

> **Tip:** Put tunable variables in `defaults/main.yml`.  
> Avoid secrets inside the role—use **Ansible Vault** externally.  
> Use **handlers** for service restarts and keep tasks **idempotent**.

### 🚀 Create the structure automatically

Generate a full skeleton with **Ansible Galaxy**:

From your repo root, create the role under ./roles
```bash
ansible-galaxy init --init-path roles my_role_name
```

Alternative (creates in the current directory):
```bash
ansible-galaxy init my_role_name
```

