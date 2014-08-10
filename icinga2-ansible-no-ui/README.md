Role Name
========

Ansible role to install Icinga2 Headless alongside Plugins

Requirements
------------

None

Role Variables
--------------

```yaml
# Vars for Debian OS Family
icinga2_key: "http://packages.icinga.org/icinga.key"
icinga2_debmon_key: "http://debmon.org/debmon/repo.key"

icinga2_deb_repos:
 - { repo: "deb http://packages.icinga.org/{{ ansible_distribution|lower }} icinga-{{ ansible_distribution_release }} main" }
 - { repo: "deb-src http://packages.icinga.org/{{ ansible_distribution|lower }} icinga-{{ ansible_distribution_release }} main" }
icinga2_debmon_repo: "deb http://debmon.org/debmon debmon-wheezy main"

icinga2_pkg:
 - { package: "icinga2" }
 - { package: "nagios-plugins" }

# Vars for RH OS Family
req_pkg:
 - { package: "gpg" }

epel6_url: "http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm"
epel7_url: "http://dl.fedoraproject.org/pub/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm"

icinga2_url_yum: "http://packages.icinga.org/epel/ICINGA-release.repo"
icinga2_url_yum_fedora: "http://packages.icinga.org/fedora/ICINGA-release.repo"
icinga2_repo_yum: "/etc/yum.repos.d/ICINGA-release.repo"

icinga2_yum:
 - { package: "icinga2" }
 - { package: "nagios-plugins-all" }

```


Dependencies
------------

None


Example Playbook
-------------------------

```yaml
    - hosts: MonitoringServers
      roles:
         - { role: icinga2-ansible-no-ui,
                   tags: ["icinga2-no-ui"] }
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
