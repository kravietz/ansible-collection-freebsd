# Ansible Collection - krvtz.freebsd

Collection of roles to configure FreeBSD servers:

* `krvtz.freebsd.conf` — basic system configuration such as `/etc/rc.conf, sysctl.conf` etc
* `krvtz.freebsd.pf` — management of `pf` firewall with multi-level Ansible group and host rules
* `krvtz.freebsd.ipfw` — management of `ipfw` FreeBSD native firewall

Example playbook using all three roles: [test.yaml](tests/test.yaml).

## Acknowledgements

Architecture of the `pf` role has been inspired by [ipr-cnrs/nftables](https://github.com/ipr-cnrs/nftables),
which in turn was inspired by [mikegleasonjr/ansible-role-firewall](https://github.com/mikegleasonjr/ansible-role-firewall).