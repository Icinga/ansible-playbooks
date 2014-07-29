Role Name
========

Ansible role to install Icinga2 Headless

Requirements
------------

None

Role Variables
--------------

See `vars/`

Dependencies
------------

Example Playbooks
-------------------------

Install Icinga2 Headless:

      - hosts: monitoring
        roles:
         - { role: icinga2-ansible,
                   tags: ["icinga2"] }

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
