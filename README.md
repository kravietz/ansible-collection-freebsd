# Ansible Collection - kravietz.freebsd

Collection of roles to configure FreeBSD servers:

## kravietz.freebsd.config
Role `kravietz.freebsd.config` — essential system configuration: `/etc/rc.conf`, `sysctl.conf`,
`/boot/loader.conf`, custom config files and directories.

* Documentation [roles/config/README.md][roles/config/README.md]
* Variables: [roles/config/defaults/main.yaml](roles/config/defaults/main.yaml)
* Example: [tests/config.yaml](tests/config.yaml)

## kravietz.freebsd.pf
Role `kravietz.freebsd.pf` — manage `pf` firewall with multi-level Ansible group and host rules.

* Variables: [roles/pf/defaults/main.yaml](roles/pf/defaults/main.yaml)
* Example: [tests/pf.yaml](tests/pf.yaml)

## kravietz.freebsd.ipfw
Role `kravietz.freebsd.ipfw` — manage `ipfw` FreeBSD native firewall.

* Variables: [roles/ipfw/defaults/main.yaml](roles/ipfw/defaults/main.yaml)
* Example: [tests/ipfw.yaml](tests/ipfw.yaml)

## Acknowledgements

The code for merging variables across groups has been inspired by [ipr-cnrs/nftables](https://github.com/ipr-cnrs/nftables),
which in turn was inspired by [mikegleasonjr/ansible-role-firewall](https://github.com/mikegleasonjr/ansible-role-firewall).