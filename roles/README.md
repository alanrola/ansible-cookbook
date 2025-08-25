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
