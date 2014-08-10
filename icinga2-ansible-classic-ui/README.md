Role Name
========

Ansible role to install Icinga2 Classic UI

Dependencies
------------

Roles: **icinga2-ansible-no-ui**

Example Playbook
-------------------------

```yaml
    - hosts: monitoring_servers
      roles:
       - { role: icinga2-ansible-classic-ui,
                 icinga2_classic_ui_passwd: "CHANGEME",
                 tags: ["icinga2-classic-ui"] }
```


Role Variables
--------------

```yaml
icinga2_classic_ui_passwd: "none"

# Vars for Debian OS Family
icinga2_classic_ui_pkg:
 - { package: "icinga2-classicui" }
 - { package: "python-passlib" }

htpasswd_deb: "/etc/icinga2/classicui/htpasswd.users"

# Vars for RH OS Family
icinga2_classic_ui_rpm:
 - { package: "icinga2-classicui-config" }
 - { package: "icinga-gui" }
 - { package: "python-passlib" }

htpasswd_rh: "/etc/icinga/passwd"
```
License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team

