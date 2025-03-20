NHC
===

This role installs and configures the [NHC][1] software.

[1]: https://github.com/mej/nhc

Role Variables
--------------

See [defaults/main.yml](defaults/main.yml) for the list of variables.

Example Playbook
----------------

Install and configure NHC:

    - hosts: computes
      roles:
        - role: btravouillon.nhc
          tags: 'role::nhc'
