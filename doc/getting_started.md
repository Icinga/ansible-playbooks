### Getting started with Icinga2 Ansible role

Install the Icinga2 role:

`ansible-galaxy install Icinga.icinga2-ansible`

Now is time to prepare your inventory.

For this moment you may want to have 2 headless monitoring servers. Create an inventory file with your preferred text editor:

```
[monitors_icinga2_core]
mnt_srv1 ansible_ssh_host="" ansible_ssh_user=""
mnt_srv2 ansible_ssh_host="" ansible_ssh_user=""
```

Then create a simple playbook, `site.yml` and place the following content:

```
- hosts: monitors_icinga2_core
  
  roles:
   - { role: Icinga.icinga2-ansible, icinga2_classicui: "no" }
```

then launch the playbook:

`ansible-playbook -i inventory site.yml`

What happens if you want to setup 2 monitoring servers with Icinga2 Classic UI alongside the previous two? Add them in your inventory:

[monitors_icinga2_core]
mnt_srv1 ansible_ssh_host="" ansible_ssh_user=""
mnt_srv2 ansible_ssh_host="" ansible_ssh_user=""

[monitors_icinga2_classicui]
mnt_srv3 ansible_ssh_host="" ansible_ssh_user=""
mnt_srv4 ansible_ssh_host="" ansible_ssh_user=""

Given that Classic UI for Debian OS family requires a password which will be passed to Debconf, open up `vars/Ubuntu_trusty.yml` and `vars/Debian_wheezy.yml` and fill `icinga2_classicui_password: "CHANGEME"` with your desired password.

Then update your playbook:

```
- hosts: monitors_icinga2_core
  
  roles:
   - { role: Icinga.icinga2-ansible, icinga2_classicui: "no" }

- hosts: monitors_icinga2_classicui
  
  roles:
   - { role: Icinga.icinga2-ansible, icinga2_classicui: "yes" }
```

and then launch again the playbook:

`ansible-playbook -i inventory site.yml`
