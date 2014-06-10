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
