## Setting up Icinga 2 with Ansible roles

Assuming that you have Ansible already installed and configured, move to a directory of your choice and clone the **icinga2-ansible** repo:

`git clone https://github.com/Icinga/icinga2-ansible.git`

Append the path where you have cloned the repository to your **.ansible.cfg roles_path**:

`roles_path=/etc/ansible/roles:~/icinga2-ansible`

Create an inventory file and place inside your monitoring servers entries:

```ini
[monitoring_servers]
icinga_one ansible_ssh_host=1.2.3.4
icinga_two ansible_ssh_host=5.6.7.8
```

Now create a playbook to hold instructions for Ansible, let's call this file `site.yml`:

```yaml
---
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
```

Save the file and close, then launch the playbook with Ansible:

`ansible-playbook site.yml -i inventory.ini`

During the execution the role will take care of all the tasks required to install and configure Icinga2 with Plugins. Here's a list of the tasks executed by **icinga2-ansible-no-ui** role:

* Get Icinga2 Apt Key for Debian OS family 
* Get Debmon Apt Key for Debian OS family (Debian) 
* Get Icinga2 Apt Repos for Debian OS family 
* Get Debmon Apt Repo for Debian OS family (Debian) 
* Install Icinga2 on Debian OS family 
* Install Requirements on RedHat OS family (Fedora) 
* Enable Epel on RedHat OS family (RH6) 
* Enable Epel on RedHat OS family (RH7) 
* Get Icinga2 Yum Key on RedHat OS family 
* Get Icinga2 Yum Repo on RedHat OS family (Fedora) 
* Get Icinga2 Yum Repo on RedHat OS family 
* Install Icinga2 on RedHat OS family 
* Start Icinga2 
* Copy Main Icinga2 Configuration 

After Icinga2 is installed you can move forward to adding a Web UI. Open up the playbook with your favorite text editor and add the role for Classic UI:

```yaml
---
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

   - { role: icinga2-ansible-classic-ui,
             icinga2_classic_ui_passwd: "CHANGEME",
             tags: ["icinga2-classic-ui"] }

```

Make sure  to replace CHANGEME with your desidered password for user icingaadmin.

Save the file and close, then launch the playbook with Ansible as usual:

`ansible-playbook site.yml -i inventory.ini`

During the execution the role will take care of all the tasks required to install and configure Icinga2 Classic UI. Please take note that **icinga2-ansible-classic-ui** does not install Icinga2, and must be used after **icinga2-ansible-no-ui**. Here's a list of the tasks executed by **icinga2-ansible-classic-ui** role:

* Install Icinga Classic UI on Debian OS family 
* Configure a password for icingaadmin user 
* Install Icinga Classic UI on RedHat OS family 
* Configure a password for icingaadmin user 
