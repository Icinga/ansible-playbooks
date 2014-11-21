Role Name
========

Ansible role to install and configure Nrpe Agent and Plugins

Dependencies
------------

None

Requirements
------------

EPEL

Example Playbook
-------------------------

```yaml
---
- hosts: all
  gather_facts: True
  roles:
   - { role: icinga2-nrpe-agent,
              nrpe_check_commands:
              { check_load: { check_load: "-w 15,10,5 -c 30,25,15" },
                check_ssh: { check_ssh: "-H {{ ansible_lo.ipv4.address }} -p {{ ansible_ssh_port }}" },
                check_disk: { check_disk: "-w 15% -c 10% -p / -p /home -p /tmp", }},
             tags: ["nrpe-agent"] }
```

Role Variables
--------------

```yaml
nrpe_agent_RedHat:
 - { package: "nrpe" }
 - { package: "nagios-plugins-all" }

nrpe_agent_Debian:
 - { package: "nagios-nrpe-server" }
 - { package: "nagios-nrpe-plugin" }
 - { package: "nagios-plugins" }

nrpe_configuration:
 - { directive: "log_facility=daemon" }
 - { directive: "server_port=5666" }
 - { directive: "server_address={{ ansible_default_ipv4.address }}" }
 - { directive: "dont_blame_nrpe=0" }
 - { directive: "debug=0" }
 - { directive: "command_timeout=60" }
 - { directive: "connection_timeout=150" }

nrpe_agent_config: "/etc/nagios/nrpe.cfg"

# Plugins path for Debian x86_64 and other i386
nrpe_agent_plugins: "/usr/lib/nagios/plugins"

# Plugins path for RH and other x86_64
nrpe_agent_plugins_x86_64: "/usr/lib64/nagios/plugins"

# Base NRPE check commands
nrpe_check_commands:
  check_load:
    check_load: "-w 15,10,5 -c 30,25,20"
  check_disk:
    check_disk: "-w 20% -c 10% -p /"

# Specific NRPE check commands for Debian
nrpe_check_commands_Debian:
  check_apt:
    check_apt: "#Works fine without arguments"
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
