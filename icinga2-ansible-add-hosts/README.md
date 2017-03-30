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
     icinga_host_attributes:
       check_command: "http"
       vars.sla: "24x7"
     icinga_host_templates:
       - icinga/sensors.j2
     host_checks: |
       object Service "load_average" {
         host_name = "{{ hostvars[item]['ansible_fqdn'] }}"
         check_command = "check_nrpe"
         vars.remote_nrpe_command = "check_load"
       }

       object Service "disk" {
         host_name = "{{ hostvars[item]['ansible_fqdn'] }}"
         check_command = "check_nrpe"
         vars.remote_nrpe_command = "check_disk"
       }
     tags: add-hosts

```

The `icinga_host_templates` allows you to inject more complex
`host_variables` through a template.
In this example, you would add `templatse/icinga/sensors.j2`, e.g.

```
vars.sensors = {
    {% for sensor in hostvars[item].sensors %}
    "Sensor {{ sensor.name }}" = {
        sensor_name = "{{ sensor.name }}"
        cpu_temp_crit = {{ sensor.critical }}
        cpu_temp_warn = {{ sensor.warning }}
    },
    {% endfor %}
}
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
