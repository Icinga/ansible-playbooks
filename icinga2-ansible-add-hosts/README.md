Role Name
========

Ansible role to add Host Definitions to Icinga2

Dependencies
------------

Roles: **icinga2-ansible-no-ui**

Example Playbook
-------------------------

```yaml
---
  # Contact the monitored hosts to gather facts
- hosts: all
  gather_facts: True

  # Contact the monitoring servers to copy host definitions
- hosts: monitoring_servers
  roles:
   - { role: icinga2-ansible-no-ui,
             tags: ["icinga2-no-ui"] }

   - { role: icinga2-ansible-add-hosts,
              configuration_logic: "object",
              tags: ["add-hosts"] }
```

Role Variables
--------------

```yaml
icinga2_hosts_dir: "/etc/icinga2/conf.d/hosts"
```
License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
