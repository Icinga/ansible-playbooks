Role Name
=========

Ansible role to install Icinga2 Web2 Ui

Requirements
------------

Mysql or MariaDB - Httpd or Nginx - PHP

Dependencies
------------

Roles: **icinga2-ansible-no-ui**

Example Playbook
----------------

```yaml
---
- hosts: MonitoringServers
  roles:

   - role: icinga2-ansible-no-ui
     icinga2_conf_global: |
       include "constants.conf"
       include "zones.conf"
       include <itl>
       include <plugins>
       include "features-enabled/*.conf"
       include_recursive "conf.d"
     check_commands:
       check_nrpe: |
          "-H", "$address$",
              "-c", "$remote_nrpe_command$",
     tags: icinga2-no-ui

   - role: icinga2-ansible-web2-ui
     icinga2_web2_db_pass: "CHANGEME"
     icinga2_ido_mysql_configuration: |
       library "db_ido_mysql"

       object IdoMysqlConnection "ido-mysql" {
         user = "{{ icinga2_web2_db_user }}"
         password = "{{ icinga2_web2_db_pass }}"
         host = "localhost"
         database = "{{ icinga2_web2_db }}"
       }
     tags: icinga2-ansible-web2-ui

```

Role Variables
--------------

See `defaults/main.yml`

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team

