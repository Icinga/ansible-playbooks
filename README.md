Role Name
========

Ansible role to install Icinga2

Requirements
------------

None

Role Variables
--------------

The variables that can be passed to this role:

| Name			|Possible values |
|-----------------------|----------------|
|icinga2_classicui: ""	| yes/no         |

The variables that you are required to configure:

File `vars/Ubuntu_trusty.yml` and `vars/Debian_wheezy.yml`

icinga2_classicui_password: "CHANGEME"

Dependencies
------------

TODO

Example Playbooks
-------------------------

```
- hosts: monitors
  
  roles:
   - { role: icinga2-ansible, icinga2_classicui: "no" }
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
