Role Name
========

Ansible role to install Icinga2 Headless alongside Plugins

Requirements
------------

EPEL

Example Playbook
-------------------------

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
