# install_node_exporter

Installs **Prometheus node_exporter** and runs it as a systemd service.

> Simple, idempotent role. Review variables before using in production.

---

## 🔧 Variables

<!-- HTML table for crisp rendering -->
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>node_exporter_version</code></td>
      <td><code>"1.8.2"</code></td>
      <td>Node Exporter version to install.</td>
    </tr>
    <tr>
      <td><code>node_exporter_download_url</code></td>
      <td><code>https://github.com/prometheus/node_exporter/releases/.../node_exporter-&lt;version&gt;.linux-amd64.tar.gz</code></td>
      <td>Source tarball URL.</td>
    </tr>
    <tr>
      <td><code>node_exporter_checksum</code></td>
      <td><code>""</code></td>
      <td>Optional checksum (<code>sha256:...</code>) for verification.</td>
    </tr>
    <tr>
      <td><code>node_exporter_user</code></td>
      <td><code>node_exporter</code></td>
      <td>System user that runs the service.</td>
    </tr>
    <tr>
      <td><code>node_exporter_bin_dir</code></td>
      <td><code>/usr/local/bin</code></td>
      <td>Install directory for the binary.</td>
    </tr>
    <tr>
      <td><code>node_exporter_listen_address</code></td>
      <td><code>0.0.0.0:9100</code></td>
      <td>Listen address for metrics.</td>
    </tr>
    <tr>
      <td><code>node_exporter_collectors_enabled</code></td>
      <td><code>[ "systemd" ]</code></td>
      <td>Collectors to enable (adds <code>--collector.&lt;name&gt;</code> flags).</td>
    </tr>
    <tr>
      <td><code>node_exporter_extra_flags</code></td>
      <td><code>[]</code></td>
      <td>Additional flags to append to ExecStart.</td>
    </tr>
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
