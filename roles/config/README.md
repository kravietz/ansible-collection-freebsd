Role Name
=========

Configure key FreeBSD server settings and drop arbitrary configuration files.

Role Variables
--------------
**Note:** Variables marked with `⛙` will be merged from global, group and host level variables into a single, final dictionary.

### `freebsd_pkg_channel`

[pkg](https://man.freebsd.org/cgi/man.cgi?query=pkg&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) [update branch](https://wiki.freebsd.org/Ports/QuarterlyBranch), can be `quarterly` (default) or `latest`.

### `freebsd_update_enable`

Boolean. Whether [freebsd-update cron](https://man.freebsd.org/cgi/man.cgi?query=freebsd-update&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html) should be run periodically from `cron`. Default `false`. This will not actually upgrade the system, only check for updates
and send them over email to root, if available.

### `freebsd_update_binary`

String. If undefined or `freebsd-update` use the default `freebsd-update` binary. If `freebsd-rustdate`, use the [freebsd-rustdate](https://rustdate.over-yonder.net) alternative to `freebsd-update`.

### `freebsd_pkg_upgrade_security`

Boolean. Run [pkg upgrade --vulnerable](https://man.freebsd.org/cgi/man.cgi?query=pkg-upgrade#end) daily to upgrade vulnerable packages. Default `false`. This will actually upgrade packages but not restart services.

### `freebsd_pkg_upgrade_all`

Boolean. Run [pkg upgrade](https://man.freebsd.org/cgi/man.cgi?query=pkg-upgrade#end) daily to upgrade all packages. Default `false`. This will actually upgrade packages but not restart services.

### ⛙ `freebsd_packages_global`, `freebsd_packages_group`, `freebsd_packages_host`

List of packages to be installed using [pkg](https://man.freebsd.org/cgi/man.cgi?query=pkg&apropos=0&sektion=8&manpath=FreeBSD+13.2-RELEASE+and+Ports&arch=default&format=html). Lists from each level (global, group, host) are added.

### ⛙ `freebsd_sysctl_noreload_global`, `freebsd_sysctl_noreload_group`, `freebsd_sysctl_noreload_host`

Dictionary of [sysctl](https://man.freebsd.org/cgi/man.cgi?sysctl(8)) key names and values to be set in `/etc/sysctl.conf` only (they won't be set in the kernel until the next reboot).

Example:

```
freebsd_sysctl_noreload_global:
  kern.randompid: 1
```

### ⛙ `freebsd_sysctl_global`, `freebsd_sysctl_group`, `freebsd_sysctl_host`

Dictionary of [sysctl](https://man.freebsd.org/cgi/man.cgi?sysctl(8)) key names and values to be set in `/etc/sysctl.conf` and in kernel.

Example:

```
freebsd_sysctl_global:
  net.inet6.ip6.use_tempaddr: 0
```

### ⛙ `freebsd_sysrc_global`, `freebsd_sysrc_group`, `freebsd_sysrc_host`

Dictionary of [rc.conf](https://man.freebsd.org/cgi/man.cgi?query=rc.conf&sektion=5&apropos=0&manpath=FreeBSD+13.2-RELEASE+and+Ports) key names and values to be set using [sysrc](https://man.freebsd.org/cgi/man.cgi?query=sysrc&sektion=8&format=html).

Example:

```
freebsd_sysrc_global:
  rtsold_enable: "YES"
  ip6addrctl_policy: "ipv6_prefer"
```

### ⛙ `freebsd_loader_global`, `freebsd_loader_group`, `freebsd_loader_host`

Dictionary of to be set in key names and values [loader.conf](https://man.freebsd.org/cgi/man.cgi?query=loader.conf&sektion=5&apropos=0&manpath=FreeBSD+13.1-RELEASE+and+Ports).

Example:

```
freebsd_loader_global:
  security.bsd.allow_destructive_dtrace: 0
```


### ⛙ `freebsd_configs_global`, `freebsd_configs_group` and `freebsd_configs_host`

Dictionaries dropping arbitrary configuration files in the system.

File contents defined inline using `content` attribue (Ansible variables like `{{ value }}` are supported):

```
freebsd_configs_global:
  test file 1:
    owner: www
    group: www
    mode: "0640"
    dest: /usr/local/etc/test1.txt
    content: test 1
    restart: nginx # optional
```

The `restart` attribute is optional and can be used to specify a service to be restarted after the file is updated. Ansible [service](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html#parameter-state) module will be used to restart the service.

File contents defined using Jinja2 [Ansible templates](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) using `src` attribute:

```
freebsd_configs_global:
  test file 2:
    owner: www
    group: www
    mode: "0640"
    src: templates/file2.j2
    content: test 1
```

**Warning:** The `content` and `src` attributes are mutually exclusive.

File contents defined inline using YAML rendered into well-formatted YAML:

```
freebsd_configs_global:
  test file 3:
    owner: www
    group: www
    mode: "0640"
    format: yaml
    content:
      key: value
      nested:
        structure:
          array:
            - one
            - two
```

File contents defined inline using YAML rendered into well-formatted JSON:

```
freebsd_configs_global:
  test file 4:
    owner: www
    group: www
    mode: "0640"
    format: json
    content:
      key: value
      nested:
        structure:
          array:
            - one
            - two
```

### ⛙ `freebsd_users_global`, `freebsd_users_group`, `freebsd_users_host`

Dictionary of users to be added or removed from the system.

Example:

```
# group_vars/all.yaml
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

# group_vars/group1.yaml
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
  roles:
    - kravietz.freebsd.config
```

License
-------

GPL-3.0-or-later

Author Information
------------------

Paweł Krawczyk https://krvtz.net/
