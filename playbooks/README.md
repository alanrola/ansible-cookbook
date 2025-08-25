# 📂 Playbooks

This folder contains a collection of **Ansible playbooks**, written as reusable and easy-to-understand “recipes”.
> **Note:** These examples are meant for learning and sharing practices. Review and adapt before using in production.

---

## Available Playbooks

<!-- Tabla HTML para mejor control visual -->
<table>
  <thead>
    <tr>
      <th>📄 Playbook</th>
      <th>📝 Description</th>
      <th>▶️ Run example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>change_symb_link.yml</code></td>
      <td>Updates the Java symbolic link to point to the desired JDK version.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/change_symb_link.yml -e "source=/usr/java/jdk1.8.0_381 dest=/usr/java/latest owner=root group=root"</code></td>
    </tr>
    <tr>
      <td><em>(coming soon)</em></td>
      <td>More recipes will be added here.</td>
      <td>—</td>
    </tr>
  </tbody>
</table>
