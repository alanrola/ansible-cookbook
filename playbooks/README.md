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
      <td><a href="./playbooks/change_symb_link.yml">change_symb_link.yml</a></td>
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
      <td><code>configure-cron.yml</code></td>
      <td>Creates/updates a cron entry under <code>/etc/cron.d</code> using the Ansible cron module.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/configure-cron.yml -e "name=nightly_backup minute=0 hour=3 day=* month=* weekday=* user=root job='/usr/local/bin/backup.sh' cron_file=nightly_backup"</code></td>
    </tr>
    <tr>
      <td><code>install_scap_client.yml</code></td>
      <td>Enables Satellite Client repo and installs <code>rubygem-foreman_scap_client</code> and Katello host tools.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/install_scap_client.yml</code></td>
    </tr>
    <tr>
      <td><code>clean_path.yml</code></td>
      <td>Cleans up old files under a target path based on age, size, and filename patterns.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/clean_path.yml -e "target_host=all target_path=/var/log age_days=14 max_file_size_mb=200"</code></td>
    </tr>
    <tr>
      <td><code>refresh_expire_user.yml</code></td>
      <td>Resets password change date to today and sets expiration policy for a given user on RHEL.</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/refresh_expire_user.yml -e "usuario=myuser dias_expiracion=90"</code></td>
    </tr>
    <tr>
      <td><code>list_pods.yml</code></td>
      <td>Lists all Kubernetes Pods cluster-wide and prints a compact table (namespace, name, ready, status, restarts, age, IP, node).</td>
      <td><code>ansible-playbook -i localhost, playbooks/list_pods.yml</code></td>
    </tr>
    <tr>
      <td><code>insights-client_7122649.yml</code></td>
      <td>Cleans Insights cache files, re-registers the client, prints version, and restarts the service (per Red Hat remediation 7122649).</td>
      <td><code>ansible-playbook -i inventory/hosts.ini playbooks/insights-client_7122649.yml</code></td>
    </tr>
  </tbody>
</table>
