## Adding Host Definitions on Icinga 2 with Ansible roles

After Icinga2 is installed (either headless or alongside a WEB UI), you can move forward to host definitions.

Given that this role will iterate over all hosts in all groups, you are encouraged to split your monitored hosts in multiple inventories.

For instance suppose that you want to monitor a dbserver: create a new inventory file, with your monitoring server entry in place, and begin to add an host to be monitored:

```ini
[monitoring_servers]
icinga2server ansible_ssh_host=1.2.3.4

[dbservers]
dbserver_one ansible_ssh_host=9.0.1.1
```

Now open up your `site.yml` playbook with your favorite text editor and add the role **icinga2-ansible-add-hosts**:

```yaml
---
  # Contact all the monitored hosts to gather facts
- hosts: all
  gather_facts: True

  # Contact all the monitoring servers to copy host definitions
- hosts: monitoring_servers
  roles:
   - { role: icinga2-ansible-no-ui,
             tags: ["icinga2-no-ui"] }

   - { role: icinga2-ansible-classic-ui,
             icinga2_classic_ui_passwd: "CHANGEME",
             tags: ["icinga2-classic-ui"] }

   - { roles: icinga2-ansible-add-hosts,
              configuration_logic: "object",
              tags: ["add-hosts"] }
```

As you have noticed, there is a new variable in place: **configuration_logic**.

When configuration_logic is "object", the role will generate host definitions following an [Object logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#configuration-best-practice)
 
**This role feature is still in development** When configuration_logic variable is "apply", the role will generate host definitions following an [Apply logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#using-apply)

Given that icinga2 and Classic UI are already installed, we are interested only to _add-hosts_ tag. Please take note that icinga2-ansible-add-hosts does not install Icinga2, and must be used after icinga2-ansible-no-ui.

Save the file and close, then launch the playbook with Ansible:

`ansible-playbook site.yml -i inventory_dbservers.ini -t add-hosts`

At this point you should see host entries in `/etc/icinga2/conf.d/hosts`:

```bash
# ls -1 /etc/icinga2/conf.d/hosts/

host_centos7icinga.conf
host_ubuntudb.conf
```

Each entry should look like this:

```bash
# cat /etc/icinga2/conf.d/hosts/host_ubuntudb.conf 

object Host "ubuntudb" {
  import "generic-host"
  address = "9.0.1.1"
  vars.sla = "24x7"
  check_command = "hostalive"
}
```

Now that we have our dbserver monitored, you may want to add some webservers to your monitored hosts in Icinga2. Create an additional inventory, and with your monitoring server definition still in place add your webservers to be monitored:

```ini
[monitoring_servers]
icinga2server ansible_ssh_host=1.2.3.4

[webservers]
webserver_one ansible_ssh_host=8.0.1.1
webserver_two ansible_ssh_host=8.0.1.2
```

Save the file and close, then launch the playbook:

`ansible-playbook site.yml -i inventory_webservers.ini -t add-hosts`

At this point you should see more host entries in `/etc/icinga2/conf.d/hosts`:

```bash
# ls -1 /etc/icinga2/conf.d/hosts/

host_centos7icinga.conf
host_debianhttp.conf
host_fedorahttp.conf
host_ubuntudb.conf
```

Each entry should look like this:

```bash
# cat /etc/icinga2/conf.d/hosts/host_fedorahttp.conf 

object Host "fedorahttp" {
  import "generic-host"
  address = "8.0.1.1"
  vars.sla = "24x7"
  check_command = "hostalive"
}
```
