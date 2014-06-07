### Ansible playbook to install Icinga 2

![Picture](https://www.icinga.org/wp-content/uploads/2013/07/icinga2_core_logo.png)

#### Platform - Prereq - Disclaimer

Tested on: CentOS 6.5 x64 - Debian Wheezy x64 - Ubuntu Trusty x64 - Fedora 20

This playbook is intended to be run against a fresh server!

#### Usage: installation

`ansible-playbook -i inventory site.yml`

#### Usage: adding hosts

`ansible-playbook -i inventory add_hosts.yml --extra-vars="hosts=monitored"`

Please, see the [Wiki](https://github.com/valentinogagliardi/ansible-icinga2/wiki) for full documentation.

#### TODO

- [ ] add Travis CI building
- [ ] add Icinga Web installation
- [ ] add Icinga 2 Web installation

#### Credits

[Icinga2](http://www.icinga.org/icinga2/)

[Ansible](http://www.ansible.com/)

Icinga2 Playbook by <a href="https://plus.google.com/+ValentinoGagliardi?rel=author">Valentino Gagliardi </a>
