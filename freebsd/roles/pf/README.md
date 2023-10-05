Role Name
=========

Configure FreeBSD [pf](https://docs.freebsd.org/en/books/handbook/firewalls/#firewalls-pf) firewall.

Role Variables
--------------

### `pf_rules_debug`

Boolean controlling whether to dump final rules set during playbook run for debugging purposes. Default `false`.

### `pf_rules_global`, `pf_rules_group`, `pf_rules_host`

Variables storing actual `pf` rules on global, group and host level. The variables are merged across all groups, contrasting with the default Ansible behaviour (overwrite). When the host is a member of several groups, it will receive rules from _all_ of these groups.

Rules are also sorted alphabetically by the key name, which impacts the ordering of output rules in the final `/etc/pf.conf`. This allows very precise ordering of rules even in case of complex architectures with many groups and hosts. It is recommended that a consistent ranges are used across all projects, for example ranges 000-100 and 900-999 for global rules, 101-499 for group rules and 500-899 for host-level rules.

Examples:

```
# host_vars/localhost.yaml
pf_rules_host:
  500 nfs:
    - nfs="{ sunrpc, nfsd-status, nfsd-keepalive, nfsd, lockd, 2050 }"
    - pass in quick inet6 proto { tcp, udp } from 2a02:390:79ef::/48 to any port $nfs

# group_vars/all.yaml
pf_rules_global:
  900 policy:
    - pass in log
  000 lo0:
    - set skip on lo0

# group_vars/group1.yaml
pf_rules_group:
  101 rules for group1:
    - pass in quick inet  proto tcp from 192.168.0.0/16 to any port = ssh

# group_vars/group2.yaml
pf_rules_group:
  201 rules for group2:
    - "pass in quick inet6 proto tcp from { 2001:db8::/32 } to any port = ssh"
```

This will produce the following `pf.conf`:

```
# 000 lo0
set skip on lo0
# 101 rules for group1
pass in quick inet  proto tcp from 192.168.0.0/16 to any port = ssh
# 201 rules for group2
pass in quick inet6 proto tcp from { 2001:db8::/32 } to any port = ssh
# 500 nfs
nfs="{ sunrpc, nfsd-status, nfsd-keepalive, nfsd, lockd, 2050 }"
pass in quick inet6 proto { tcp, udp } from 2a02:390:79ef::/48 to any port $nfs
# 900 policy
pass in log
```

Example Playbook
----------------

Assuming the above example variables configured in respective group and host files:

```
- hosts: localhost
  become: true
  become_user: root
  name: Test
  roles:
    - kravietz.freebsd.pf
```

License
-------

GPL-3.0-or-later

Author Information
------------------

Paweł Krawczyk https://krvtz.net/