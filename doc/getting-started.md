## Setting up Icinga 2 with Ansible roles

### Setting up Icinga2 headless

Assuming that you have Ansible already installed and configured, move to a directory of your choice and clone the **icinga2-ansible** repo:

`git clone https://github.com/Icinga/icinga2-ansible.git`

Append the path where you have cloned the repository to your **.ansible.cfg** _roles_path_. In this example we are assuming that you have cloned the repo in your home directory:

`roles_path=/etc/ansible/roles:~/icinga2-ansible`

Create an inventory file and place your monitoring servers entries inside:

```ini
[monitoring_servers]
icinga_one ansible_ssh_host=1.2.3.4
```

Now create a playbook to hold instructions for Ansible, let's call this file `site.yml`:

```yaml
---
- hosts: monitoring_servers
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

Save the file and close, then launch the playbook with Ansible:

`ansible-playbook site.yml -i inventory.ini`

During the execution the role will take care of all the tasks required to install and configure Icinga2 with Plugins.

### Setting up Icinga2 alongside Classic UI

After Icinga2 is up and running you can move forward to add a Classic UI. Open up the playbook with your favorite text editor and add the role for Classic UI:

```yaml
---
- hosts: monitoring_servers
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

   - role: icinga2-ansible-classic-ui
     icinga2_classic_ui_passwd: 'CHANGEME'
     tags: icinga2-classic-ui
```

Make sure  to replace CHANGEME with your desidered password for user **icingaadmin**.

Save the file and close, then launch the playbook with Ansible as usual:

`ansible-playbook site.yml -i inventory.ini`

During the execution the role will take care of all the tasks required to install and configure Icinga2 Classic UI. Please take note that **icinga2-ansible-classic-ui** does not install Icinga2, and must be used after **icinga2-ansible-no-ui**.

### Setting up Icinga2 alongside Web UI

After Icinga2 is up and running you can move forward to add a Web UI. Open up the playbook with your favorite text editor and add the role for Web UI:

```yaml
---
- hosts: monitoring_servers
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

   - role: icinga2-ansible-web-ui
     icinga2_web_ui_ido: "mysql"
     tags: icinga2-web-ui
```

At this moment, the role have only support for Mysql IDO Backend.

Save the file and close, then launch the playbook with Ansible as usual:

`ansible-playbook site.yml -i inventory.ini`

During the execution the role will take care of all the tasks required to install and configure Icinga2 Web UI. Please take note that **icinga2-ansible-web-ui** does not install Icinga2, and must be used after **icinga2-ansible-no-ui**.

After Icinga2 is up and running with your favorite UI, you can move forward to [add host checks](https://github.com/Icinga/icinga2-ansible/blob/master/doc/adding-hosts.md).
