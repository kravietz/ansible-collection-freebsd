# Ansible Collection - kravietz.freebsd

Collection of roles to configure FreeBSD servers:

## kravietz.freebsd.config
Role `kravietz.freebsd.config` — essential system configuration: `/etc/rc.conf`, `sysctl.conf`,
`/boot/loader.conf`, custom config files and directories.

* Documentation [freebsd/roles/config/README.md](freebsd/roles/config/README.md)
* Variables: [freebsd/roles/config/defaults/main.yaml](freebsd/roles/config/defaults/main.yaml)
* Example: [freebsd/tests/config.yaml](freebsd/tests/config.yaml)

## kravietz.freebsd.pf
Role `kravietz.freebsd.pf` — manage `pf` firewall with multi-level Ansible group and host rules.

* Documentation [freebsd/roles/pf/README.md](freebsd/roles/pf/README.md)
* Variables: [freebsd/roles/pf/defaults/main.yaml](freebsd/roles/pf/defaults/main.yaml)
* Example: [freebsd/roles/tests/pf.yaml](freebsd/tests/pf.yaml)

## kravietz.freebsd.ipfw
Role `kravietz.freebsd.ipfw` — manage `ipfw` FreeBSD native firewall.

* Documentation [freebsd/roles/ipfw/README.md](freebsd/roles/ipfw/README.md)
* Variables: [freebsd/roles/ipfw/defaults/main.yaml](freebsd/roles/ipfw/defaults/main.yaml)
* Example: [freebsd/freebsd/tests/ipfw.yaml](freebsd/freebsd/ttests/ipfw.yaml)

## Acknowledgements

The code for merging variables across groups has been inspired by [ipr-cnrs/nftables](https://github.com/ipr-cnrs/nftables),
which in turn was inspired by [mikegleasonjr/ansible-role-firewall](https://github.com/mikegleasonjr/ansible-role-firewall).