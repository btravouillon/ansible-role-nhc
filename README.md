# Ansible Role: NHC (Node Health Check)

This role installs and configures the [NHC (Node Health Check)][1] software,
which is a tool for monitoring and checking the health of compute nodes in a
cluster environment.

[1]: https://github.com/mej/nhc

## Description

This role provides a way to:
- Install NHC package
- Configure NHC through its configuration file
- Set up NHC environment variables
- Support package upgrades when needed

## Requirements

- Ansible 2.16 or higher
- Target systems must be running a supported Linux distribution (Debian/Ubuntu)

## Role Variables

Available variables are listed below. See `defaults/main.yml` for default
values.

```yaml
# Package name to install
nhc_packages: nhc

# Whether to upgrade the package (true/false)
nhc_upgrade: false

# NHC configuration content
# Example:
# nhc_config: |
#   * || check_fs_mount_rw /
#   * || check_hw_cpuinfo 2 8 8
#   * || check_hw_physmem 1k 1TB

# List of NHC environment variables
# See https://github.com/mej/nhc?tab=readme-ov-file#supported-variables
# Example:
# nhc_variables:
#   - NHC_RM=slurm
#   - HELPERDIR=/opt/nhc/libexec/nhc/

# List of NHC custom scripts
# `src` is mandatory
# `dest` and `state` are optional
nhc_scripts:
  # Add a custom script
  - src: "{{ inventory_dir }}/files/nhc/custom_script.nhc"
  # Add a script to custom INCDIR
  - src: "{{ inventory_dir }}/files/nhc/custom_script_foo.nhc"
    dest: '/opt/nhc/scripts/custom_script_foo.nhc'
  # Remove a script
  - src: "{{ inventory_dir }}/files/nhc/custom_script_bar.nhc"
    state: absent
```

## Example Playbook

Install and configure NHC:
```yaml
- hosts: computes
  roles:
    - role: btravouillon.nhc
      tags: role::nhc
      vars:
        nhc_config: |
          # Check that / is mounted read-write.
          * || check_fs_mount_rw /
          # Check that there are 2 physical CPUs, 8 actual cores, and 8 threads
          * || check_hw_cpuinfo 2 8 8
          # Check that we have between 1kB and 1TB of physical RAM
          * || check_hw_physmem 1k 1TB
        nhc_variables:
          - NHC_RM=slurm
          - HELPERDIR=/opt/nhc/libexec/nhc/
```

## License

See the [LICENSE](LICENSE) file for details.
