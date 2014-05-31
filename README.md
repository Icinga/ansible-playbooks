### Ansible playbook to install Icinga 2

#### Platform - Prereq - Disclaimer

Tested on CentOS 6.5 x64

Requires: python-apt - Ansible >= 1.6

This playbook is intended to be run against a fresh server, not a production one!

Please note: Icinga 2 is still under active development. This playbook will be updated as new feature or installation steps will be available.

#### Usage: installation

`ansible-playbook -i inventory site.yml`

then login:

**CentOS**: `http://IP/icinga` with user **icingaadmin** and password **icingaadmin**

#### Usage: adding hosts

`ansible-playbook -i inventory add_hosts.yml`

#### TODO

- [x] expand support to Debian OS family
- [ ] add Travis CI building
- [ ] add Icinga Web installation
- [ ] add Icinga 2 Web installation

#### Credits

[Ansible](http://www.ansible.com/)

[Icinga2](http://www.icinga.org/icinga2/)

Icinga2 Playbook by <a href="https://plus.google.com/+ValentinoGagliardi?rel=author">Valentino Gagliardi </a>
