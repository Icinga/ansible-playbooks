Role Name
========

Ansible role to add Host Definitions on Icinga2

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
   - role: icinga2-ansible-add-hosts
     configuration_logic: "object"
     host_attributes: |
       check_command = "http"
       vars.sla = "24x7"
     host_checks: |
       object Service "load_average" {
         host_name = "{{ hostvars[item]['ansible_fqdn'] }}"
         check_command = "check_nrpe"
         vars.nrpe_command = "check_load"
       }

       object Service "disk" {
         host_name = "{{ hostvars[item]['ansible_fqdn'] }}"
         check_command = "check_nrpe"
         vars.nrpe_command = "check_disk"
       }
     tags: add-hosts

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
