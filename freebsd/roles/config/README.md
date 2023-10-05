Role Name
=========

Configure key FreeBSD server settings.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
**Note:** Variables marked with `⛙` will be merged from global level, group level and host level variables.

### `freebsd_pkg_channel`

[pkg](https://man.freebsd.org/cgi/man.cgi?query=pkg&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) [update branch](https://wiki.freebsd.org/Ports/QuarterlyBranch), can be `quarterly` (default) or `latest`.

### `freebsd_update_enable`

Boolean. Whether [freebsd-update cron](https://man.freebsd.org/cgi/man.cgi?query=freebsd-update&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) should be run periodically from `cron`. Default `false`.

### `freebsd_packages_global`, `freebsd_packages_group`, `freebsd_packages_host`

List of packages to be installed using [pkg](https://man.freebsd.org/cgi/man.cgi?query=pkg&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html). Lists from each level (global, group, host) are added.

### `freebsd_sysctl_noreload_global`, `freebsd_sysctl_noreload_group`, `freebsd_sysctl_noreload_host` ⛙

Dictionary of [sysctl](https://man.freebsd.org/cgi/man.cgi?sysctl(8)) key names and values to be set in `/etc/sysctl.conf` only (they won't be set in the kernel until the next reboot).

Example:

```
freebsd_sysctl_noreload_global:
  kern.randompid: 1
```

### `freebsd_sysctl_global`, `freebsd_sysctl_group`, `freebsd_sysctl_host` ⛙

Dictionary of [sysctl](https://man.freebsd.org/cgi/man.cgi?sysctl(8)) key names and values to be set in `/etc/sysctl.conf` and in kernel.

Example:

```
freebsd_sysctl_global:
  net.inet6.ip6.use_tempaddr: 0
```

### `freebsd_sysrc_global`, `freebsd_sysrc_group`, `freebsd_sysrc_host` ⛙

Dictionary of [rc.conf](https://man.freebsd.org/cgi/man.cgi?query=rc.conf&sektion=5&apropos=0&manpath=FreeBSD+13.2-RELEASE+and+Ports) key names and values to be set using [sysrc](https://man.freebsd.org/cgi/man.cgi?query=sysrc&sektion=8&format=html).

Example:

```
freebsd_sysrc_global:
  rtsold_enable: "YES"
  ip6addrctl_policy: "ipv6_prefer"
```

### `freebsd_loader_global`, `freebsd_loader_group`, `freebsd_loader_host` ⛙

Dictionary of to be set in key names and values [loader.conf](https://man.freebsd.org/cgi/man.cgi?query=loader.conf&sektion=5&apropos=0&manpath=FreeBSD+13.1-RELEASE+and+Ports).

Example:

```
freebsd_loader_global:
  security.bsd.allow_destructive_dtrace: 0
```


### `freebsd_configs_global`, `freebsd_configs_group` and `freebsd_configs_host` ⛙

Dictionaries dropping arbitrary configuration files in the system.

Example:

```
freebsd_configs_global:
  test file 1:
    owner: www
    group: www
    mode: "0640"
    dest: /usr/local/etc/test1.txt
    content: test 1
```

### `freebsd_users_global`, `freebsd_users_group`, `freebsd_users_host`

Dictionary of users to be added or removed from the system.

Example:

```
freebsd_users_global:
  - name: user1
    uid: 5000
    sudo: true
    shell: /bin/sh
    sshkeys:
      - ssh-ed25519 AAA user@test
    password: "*"
    sudo_nopassword: true
    groups: wheel
freebsd_users_group:
  - name: user2
    uid: 5001
    shell: /bin/sh
    sshkeys:
      - ssh-ed25519 AAA user@test
    password: "*"
  - name: rpi
    state: absent
```

### `freebsd_mounts`

List of [fstab](https://man.freebsd.org/cgi/man.cgi?query=fstab&sektion=&n=1) lines to be added

Example:

```
freebsd_mounts:
  - /dev/ada0sN /dos msdosfs rw 0 0
```

### `freebsd_exports`

List of [exports](https://man.freebsd.org/cgi/man.cgi?query=exports&apropos=0&sektion=0&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) lines to be added

Example:

```
freebsd_exports:
  - "V4: /"
```

### `freebsd_termcap`

Dictionary of [termcap](https://man.freebsd.org/cgi/man.cgi?query=termcap&apropos=0&sektion=5&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) entries to be added on removed.

Example:

```
freebsd_termcap:
  - name: foot
    value: 'foot|foot terminal emulator:am:bw:hs:mi:m…
  - name: foot-obsolete
    state: absent
```


Example Playbook
----------------

```
- hosts: localhost
  become: true
  become_user: root
  name: Test
  vars:
    freebsd_configs_global:
      test 1:
        owner: www
        group: www
        mode: "0640"
        dest: /usr/local/etc/test1.txt
        content: test 1
      test dir:
        dest: /tmp/test1
        state: directory
    freebsd_configs_group:
      test 2:
        dest: /usr/local/etc/test2.txt
        content: test 2
    freebsd_configs_host:
      test 3:
        dest: /usr/local/etc/test3.txt
        content: test 3
  roles:
    - kravietz.freebsd.config
```

License
-------

GPL-3.0-or-later

Author Information
------------------

Paweł Krawczyk https://krvtz.net/