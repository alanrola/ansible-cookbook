# ansible-cookbook

**Ansible Automation Cookbook** — clear, reusable playbooks and roles, explained step-by-step.

> This repository is a personal collection of Ansible examples, shared as a community resource.

---

## 🚀 What’s inside

- **Ready-to-use playbooks** — practical recipes (packages, services, patching, audits, etc.).
- **Reusable roles** — Galaxy-style structure with defaults, handlers, tasks, templates, and files.
- **Beginner-friendly docs** — each example explains _what_ it does and _how_ to run it.
- **Good practices** — idempotence, minimal privileges, variables in defaults, and tidy inventories.

<table>
  <thead>
    <tr>
      <th>📁 Folder</th>
      <th>📝 Description</th>
      <th>🔗 Docs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>playbooks/</code></td>
      <td>Standalone playbooks you can run as-is.</td>
      <td><a href="./playbooks/">Playbooks index</a></td>
    </tr>
    <tr>
      <td><code>roles/</code></td>
      <td>Reusable roles with defaults and examples.</td>
      <td><a href="./roles/">Roles index</a></td>
    </tr>
  </tbody>
</table>

---

## 🗂️ Structure (short)

```text
ansible-cookbook/
├── playbooks/      # ready-to-run YAMLs + README with a usage table
└── roles/          # Galaxy-style roles + README with a usage table
```

---

## ⚙️ Quickstart

> You only need Ansible on your control machine. Inventory examples use INI; use your preferred format.

**Run a playbook (example):**
```bash
ansible-playbook -i inventory/hosts.ini playbooks/check-openssh-version.yml
```

**Use a role in your playbook (example):**
```yaml
- hosts: all
  become: true
  roles:
    - role: manage-packages
      vars:
        packages_to_install: [vim, git]
```

More examples (and exact variables) are documented in each subfolder’s README.

---

## 🎯 Scope & Intent

- This is **not** a production library; it’s a **learning resource** with practical, readable examples.
- Each recipe prefers **clarity over cleverness**, and small building blocks over monoliths.
- Contributions are welcome: issues, feedback, or small PRs that improve readability or safety.

---

## 🛡️ Notes

- Review variables before running in sensitive environments.
- Many playbooks/roles target **RHEL/Rocky/Alma 8/9**; platform-specific notes are called out per item.
