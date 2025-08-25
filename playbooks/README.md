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
      <td><code>check-openssh-version.yml</code></td>
      <td>Checks and prints the OpenSSH client version on each host.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/check-openssh-version.yml</code></td>
    </tr>
    <tr>
      <td><code>fix_uid_conflict.yml</code></td>
      <td>Resolves UID conflicts by relocating the conflicting user to a free UID and assigning the target UID to the requested user.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/fix_uid_conflict.yml -e "usuario_target=myuser uid_target=1500"</code></td>
    </tr>
    <tr>
      <td><code>modify_rhsm_conf.yml</code></td>
      <td>Updates <code>rhsm.conf</code> (port and insecure) and refreshes RHSM subscription if changes were applied.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/modify_rhsm_conf.yml</code></td>
    </tr>
    <tr>
      <td><code>change-permissions-777-to-755.yml</code></td>
      <td>Sets provided files to <code>0755</code> per host–file pairs passed via <code>server_file_list</code>.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/change-permissions-777-to-755.yml -e "mode=0755 server_file_list='host1 /path/to/file\nhost2 /another/path'"</code></td>
    </tr>
    <tr>
      <td><em>(coming soon)</em></td>
      <td>More recipes will be added here.</td>
      <td>—</td>
    </tr>
  </tbody>
</table>
