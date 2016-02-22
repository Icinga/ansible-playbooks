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
   - role: icinga2-nrpe-agent
     nrpe_allowed_hosts: "192.168.1.1,127.0.0.1"
     nrpe_configuration: |
       log_facility=daemon
       server_port=5666
       server_address={{ ansible_default_ipv4.address }}
       dont_blame_nrpe=0
       debug=0
       command_timeout=60
       connection_timeout=150
     nrpe_check_commands: |
       command[check_load]={{ nrpe_agent_plugins_x86_64 }}/check_load -w 15,10,8 -c 30,25,15
       command[check_ssh]={{ nrpe_agent_plugins_x86_64 }}/check_ssh -H {{ ansible_eth0.ipv4.address }} -p {{ ansible_port }}
       command[check_procs]={{ nrpe_agent_plugins_x86_64 }}/check_procs -w 300 -c 500
       command[check_disk]={{ nrpe_agent_plugins_x86_64 }}/check_disk -w 15% -c 10% -p / -p /home -p /tmp
     tags: nrpe-agent

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

nrpe_agent_Gentoo:
 - { package: "nrpe" }
 - { package: "nagios-plugins" }

nrpe_agent_config: "/etc/nagios/nrpe.cfg"

# A list of allowed hosts for Nrpe agent
nrpe_allowed_hosts: "127.0.0.1,192.168.0.1"

# Plugins path for RH and other x86_64
nrpe_agent_plugins_x86_64: "/usr/lib64/nagios/plugins"

# Sample NRPE check commands
nrpe_check_commands:
  check_load:
    check_load: "-w 15,10,5 -c 30,25,20"
  check_disk:
    check_disk: "-w 20% -c 10% -p /"
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
