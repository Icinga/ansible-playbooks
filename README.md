Role Name
========

Ansible role to install Icinga2

Requirements
------------

None

Role Variables
--------------

The variables that can be passed to this role in the main playbook. "Mandatory" means that you are required to specify the value of the variable.

| Name			   | Value  | Mandatory     |
|--------------------------|--------|---------------|
|icinga2_classicui:	   | yes/no | Yes	    |
|icinga2_new_web_mysql:	   | yes/no | Yes	    |
|icinga2_ido_db_user_pass: | icinga | No, only if you have icinga2_new_web_mysql: "yes"	|
|icinga2_web_db_user_pass: | icinga_web | No, only if you have icinga2_new_web_mysql: "yes"	|

The variables that you are required to configure manually:

File `vars/Ubuntu_trusty.yml` and `vars/Debian_wheezy.yml`

icinga2_classicui_password: "CHANGEME"

File `vars/CentOS_Final.yml`

icinga2_new_web_timezone: "Europe/Berlin"

Dependencies
------------

TODO

Example Playbooks
-------------------------

Install Icinga2 with Classic UI:

```
---
- hosts: monitors
  
  roles:
   - { role: icinga2-ansible, icinga2_classicui: "yes", icinga2_new_web_mysql: "no" }
```

Install Icinga2 with Icinga New Web Classic:

```
---
- hosts: monitors

  roles:
   - { role: icinga2-ansible, icinga2_classicui: "no", icinga2_new_web_mysql: "yes", icinga2_ido_db_user_pass: "icinga", icinga2_web_db_user_pass: "icinga_web"}
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
