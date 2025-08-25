# install_node_exporter

Installs **Prometheus node_exporter** and runs it as a systemd service on EL (RHEL/Rocky/Alma).

> Simple, readable, and idempotent. Review variables before using in production.

---

## 🔧 Variables

<!-- HTML table for crisp rendering on GitHub -->
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>node_exporter_version</code></td><td><code>"1.8.2"</code></td><td>Node Exporter version to install.</td></tr>
    <tr><td><code>node_exporter_os</code></td><td><code>"linux"</code></td><td>OS label used in the release tarball name.</td></tr>
    <tr><td><code>node_exporter_arch</code></td><td><code>"amd64"</code></td><td>Arch label used in the release tarball name.</td></tr>
    <tr><td><code>node_exporter_download_url</code></td><td><code>https://github.com/prometheus/node_exporter/releases/download/v{{ node_exporter_version }}/node_exporter-{{ node_exporter_version }}.{{ node_exporter_os }}-{{ node_exporter_arch }}.tar.gz</code></td><td>Release tarball URL.</td></tr>
    <tr><td><code>node_exporter_checksum</code></td><td><code>""</code></td><td>Optional checksum (e.g. <code>sha256:…</code>) to verify the download.</td></tr>
    <tr><td><code>node_exporter_bin_dir</code></td><td><code>/usr/local/bin</code></td><td>Destination directory for the binary.</td></tr>
    <tr><td><code>node_exporter_bin</code></td><td><code>{{ node_exporter_bin_dir }}/node_exporter</code></td><td>Full path for the installed binary.</td></tr>
    <tr><td><code>node_exporter_user</code></td><td><code>node_exporter</code></td><td>System user to run the service.</td></tr>
    <tr><td><code>node_exporter_listen_address</code></td><td><code>0.0.0.0:9100</code></td><td>Listen address for metrics endpoint.</td></tr>
    <tr><td><code>node_exporter_collectors_enabled</code></td><td><code>[ "systemd" ]</code></td><td>Collectors to enable (appended as <code>--collector.&lt;name&gt;</code>).</td></tr>
    <tr><td><code>node_exporter_extra_flags</code></td><td><code>[]</code></td><td>Additional flags appended to ExecStart (e.g. <code>--web.telemetry-path=/metrics</code>).</td></tr>
  </tbody>
</table>

---

## 🚀 Usage

Minimal example:
```yaml
- hosts: all
  become: true
  roles:
    - role: install_node_exporter
```

Customized:
```yaml
- hosts: all
  become: true
  roles:
    - role: install_node_exporter
      vars:
        node_exporter_version: "1.8.2"
        node_exporter_collectors_enabled: [systemd]
        node_exporter_extra_flags:
          - "--web.telemetry-path=/metrics"
```

From CLI (one-off):
```bash
ansible-playbook -i inventory/hosts.ini site.yml \
  -e 'node_exporter_version=1.8.2 node_exporter_listen_address=0.0.0.0:9100'
```

---

## 🛠 What the role does (summary)

1. Creates the system user (no shell, no home).  
2. Downloads the release tarball and unpacks it.  
3. Installs the `node_exporter` binary under `{{ node_exporter_bin_dir }}`.  
4. Templates a systemd unit at `/etc/systemd/system/node_exporter.service`.  
5. Enables and starts the service (handlers reload daemon and restart on changes).

---

## ✅ Verify

### Service status
```bash
systemctl status node_exporter
```

### Metrics locally
```bash
curl -s localhost:9100/metrics | head
```

### Listening socket
```bash
ss -ltnp | grep 9100
```

---

## 🧪 Idempotency

- Re-running the role does not reinstall if the same version is already placed at `node_exporter_bin`.  
- Service restarts only happen when the binary or unit file change.
