# Ansible Collection - krvtz.freebsd

Collection of roles to configure FreeBSD servers:

* `krvtz.freebsd.conf` — basic system configuration such as `/etc/rc.conf, sysctl.conf`, custom config files etc.
   Example: [tests/config.yaml](tests/config.yaml)
* `krvtz.freebsd.pf` — management of `pf` firewall with multi-level Ansible group and host rules.
   Example: [tests/pf.yaml](tests/pf.yaml)
* `krvtz.freebsd.ipfw` — management of `ipfw` FreeBSD native firewall.
   Example: [tests/ipfw.yaml](tests/ipfw.yaml)

## Acknowledgements

Architecture of the `pf` role has been inspired by [ipr-cnrs/nftables](https://github.com/ipr-cnrs/nftables),
which in turn was inspired by [mikegleasonjr/ansible-role-firewall](https://github.com/mikegleasonjr/ansible-role-firewall).