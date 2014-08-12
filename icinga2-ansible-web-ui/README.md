Role Name
========

Ansible role to install Icinga2 Web UI with Mysql IDO backend

Dependencies
------------

Roles: **icinga2-ansible-no-ui**

Example Playbook
-------------------------

```yaml
- hosts: monitoring_servers
  roles:
   - { role: icinga2-ansible-no-ui,
             icinga2_conf_global:
             [{ directive: 'include "constants.conf"' },
              { directive: 'include "zones.conf"' },
              { directive: 'include <itl>' },
              { directive: 'include <plugins>' },
              { directive: 'include "features-enabled/*.conf"' },
              { directive: 'include_recursive "conf.d"' }],
             tags: ["icinga2-no-ui"] }

   - { role: icinga2-ansible-web-ui,
             icinga2_web_ui_ido: "mysql",
             tags: ["icinga2-web-ui"] }
```


Role Variables
--------------

```yaml
# Vars for RH OS Family

icinga2_web_ui_rpm:
 - { package: "icinga-web" }
 - { package: "icinga-web-{{ icinga2_web_ui_ido }}" }

icinga2_web_ui_mariadb_rpm:
 - { package: "mariadb-server" }
 - { package: "MySQL-python" }

icinga2_web_ui_mysql_rpm:
 - { package: "mysql-server" }
 - { package: "MySQL-python" }

icinga2_web_db_dbuser: "icinga_web"

icinga2_ido_schema_version: "2.0.2"
icinga2_ido_mysql_schema: "/usr/share/doc/icinga2-ido-mysql-{{ icinga2_ido_schema_version }}/schema/mysql.sql"

icinga2_web_mysql_schema_rh_7: "/usr/share/doc/icinga-web-1.11.1/schema/mysql.sql"
icinga2_web_mysql_schema_rh_6: "/usr/share/doc/icinga-web-1.11.0/schema/mysql.sql"

icinga2_ido_mysql_conf: "/etc/icinga2/features-available/ido-mysql.conf"

icinga2_ido_mysql_conf_global:
 - { directive: 'user = "icinga"' }
 - { directive: 'password = "icinga"' }
 - { directive: 'host = "localhost"' }
 - { directive: 'database = "icinga"' }
```
License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team

