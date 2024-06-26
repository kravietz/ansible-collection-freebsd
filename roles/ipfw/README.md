Role Name
=========

Configure FreeBSD [ipfw](https://docs.freebsd.org/en/books/handbook/firewalls/#firewalls-ipfw) firewall.

Role Variables
--------------

### `ipfw_rules_debug`

Boolean controlling whether to dump final rules set during playbook run for debugging purposes. Default `false`.

### `ipfw_rules_global`, `ipfw_rules_group`, `ipfw_rules_host`

Variables storing actual `ipfw` rules on global, group and host level. The variables are merged across all groups, contrasting with the default Ansible behaviour (overwrite). When the host is a member of several groups, it will receive rules from _all_ of these groups.

Rules are also sorted alphabetically by the key name, which impacts the ordering of output rules in the final firewall file
installed to `/usr/local/etc/ipfw.conf`. This allows very precise ordering of rules even in case of complex architectures with many groups and hosts. It is recommended that a consistent ranges are used across all projects, for example ranges 000-100 and 900-999 for global rules, 101-499 for group rules and 500-899 for host-level rules.

Example:

```
ipfw_rules_global:
  000 default:
    # set 31 is always loaded at the end of the ruleset
    # and contains the default deny rule at the end
    - add set 31 allow tcp from 192.168.0.0/16 to me ssh
  001 lo0:
    # add new rules to an arbitrary set 8
    - set 8 flush
    - add set 8 allow ip from any to any via lo0
  101 logged drop:
    - add set 8 deny log ip from any to any
  999 swap sets:
    # swap the new rules into the default set 0
    - set swap 8 0
    - delete set 8
```

The role also sets `ipfw_type` variable in [rc.conf](https://man.freebsd.org/cgi/man.cgi?rc.conf) to the above file name, which will enable `ipfw` on boot and load the specified file. Please note that while syntax check is performed on the rules on write, logical validity of the rules is the responsibility of the user.

Example Playbook
----------------

Assuming the above example variables configured in respective group and host files:

```
- hosts: localhost
  become: true
  become_user: root
  name: Test
  roles:
    - kravietz.freebsd.ipfw
```


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
