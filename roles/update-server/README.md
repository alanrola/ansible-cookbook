# Controlled Updates for EL (7/8/9) with Reboot‑Safe Exclusions

This Ansible playbook updates **EL 7, 8, and 9** systems to the latest available packages **while excluding** those that usually require a reboot (kernel, systemd, glibc, firmware, etc.).
It also replaces the system’s YUM/DNF configuration file with a predefined version depending on the EL release.

## What It Does

1. **Configuration replacement**
   - EL 7 → copies `yum_7_exludes.conf` to `/etc/yum.conf`
   - EL 8 → copies `yum_8_exludes.conf` to `/etc/dnf/dnf.conf`
   - EL 9 → copies `yum_9_exludes.conf` to `/etc/dnf/dnf.conf`

2. **Package updates with exclusions**
   - EL 7 → uses the `yum` module
   - EL 8/9 → use the `dnf` module
   - In all cases, critical packages are excluded to minimize downtime.

> Task execution is controlled with `when: ansible_distribution_major_version == '7'/'8'/'9'`.

## Suggested Repo Layout

```
.
├── playbook.yml          # your main playbook
└── files/
    ├── yum_7_exludes.conf
    ├── yum_8_exludes.conf
    └── yum_9_exludes.conf
```
> Filenames are intentionally kept as `*_exludes.conf` to match the playbook verbatim.

## Requirements

- Ansible control node with SSH access to managed hosts.
- Remote users with `root` privileges (use `-b/--become` when running).
- Target hosts must run EL 7, 8, or 9 with fact gathering enabled.

## Compatibility Matrix

| EL Version | Module Used             | Config Destination  |
|--------------|-------------------------|---------------------|
| 7            | `ansible.builtin.yum`   | `/etc/yum.conf`     |
| 8            | `ansible.builtin.dnf`   | `/etc/dnf/dnf.conf` |
| 9            | `ansible.builtin.dnf`   | `/etc/dnf/dnf.conf` |

## Excluded Packages

To prevent unplanned reboots, the following are excluded during updates:

- **EL 7**:  
  `kernel*, *-firmware-*, glibc*, linux-firmware, dbus*, systemd*, systemd-*, udev, hal, java*, libgudev1*, openssl*, nscd*, kmod*, libnsl*`

- **EL 8/9**:  
  `kernel*, *-firmware-*, glibc*, linux-firmware, dbus*, systemd*, systemd-*, udev, hal, java*, kmod*, libnsl*`

## Example Inventory

`inventory.ini`:
```ini
[el7]
el7-01 ansible_host=10.0.0.71

[el8]
el8-01 ansible_host=10.0.0.81

[el9]
el9-01 ansible_host=10.0.0.91

```

## Usage Examples

- Run on all hosts:
```bash
ansible-playbook -i inventory.ini playbook.yml -b
```

- Limit to EL 8 only:
```bash
ansible-playbook -i inventory.ini playbook.yml -b --limit el8
```

- Dry run (check mode):
```bash
ansible-playbook -i inventory.ini playbook.yml -b -C
```

- Verbose mode:
```bash
ansible-playbook -i inventory.ini playbook.yml -b -vv
```

## Operational Notes

- **Idempotency**: The `copy` module only updates files when content differs. `yum`/`dnf` with `state: latest` ensures consistency without redundant changes.
- **No automatic reboots**: This playbook avoids reboot‑requiring packages. Schedule separate maintenance windows for kernel/systemd/glibc/firmware updates.
- **Backups**: `copy` does not enable `backup: true` by default. If rollback is critical, consider adding it in a future revision.
- **Parameter warning**: `validate_certs` is **not** a supported parameter for `ansible.builtin.copy`. If your Ansible version is strict, remove it or expect an “Unsupported parameters” error.
- **Security implications**: Excluding `openssl*` (EL 7) and other core packages defers some security updates until you explicitly include them.

## Recommended Workflow

1. Test with `-C` (check mode) on a non‑production host.
2. Validate that the `*_exludes.conf` files contain the intended directives (e.g., repo, exclude rules).
3. Apply changes on a small canary group.
4. Monitor logs and package states after execution.
5. Plan dedicated windows for the excluded packages when safe.

## Troubleshooting

- **Unsupported parameter error (`validate_certs`)**  
  Remove `validate_certs` from the `copy` task if your Ansible version does not accept it.
- **No changes applied**  
  Ensure your `src` files exist and differ from the destination paths.
- **Update failures**  
  Verify repository reachability and confirm that exclusions aren’t overly restrictive.
- **Reboot required**  
  By design, this playbook avoids such changes. If you later remove exclusions, reboot separately.

## License

State your chosen license (MIT, BSD, GPL, etc.).

## Author & Contributions

- Author: *Your Name / Team*
- Contributions welcome:
  - Add backup support
  - Parameterize exclusions
  - Implement optional reboots
  - Add tags and handlers
