## Adding Host Definitions on Icinga 2 with Ansible roles

After Icinga2 is installed (either headless or alongside a WEB UI), you can move forward to add host definitions.

Given that this role will iterate over all hosts in all groups currently in the playbook scope, you are encouraged to split your monitored hosts in multiple inventories.

For instance suppose that you want to monitor a bunch of database servers: create a new inventory file, with your monitoring server entry in place, and begin to add the hosts to be monitored:

```ini
[monitoring_servers]
icinga2server ansible_ssh_host=1.2.3.4
icinga2server_one ansible_ssh_host=1.2.3.5
icinga2server_three ansible_ssh_host=1.2.3.6

[dbservers]
dbserver_one ansible_ssh_host=9.0.1.1
dbserver_two ansible_ssh_host=9.0.1.2
dbserver_three ansible_ssh_host=9.0.1.3
dbserver_four ansible_ssh_host=9.0.1.4
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
              host_attributes:
              { vars: { vars.sla: "24x7", vars.operator: "on_call", vars.os: "{{ ansible_system }}" },
                check_command: { check_command: "hostalive" }},
        
              host_checks:
              { load_average: { check_command: "check_nrpe", vars.remote_nrpe_command: "check_load" },
                disk: { check_command: "check_nrpe", vars.remote_nrpe_command: "check_disk" },
                mysql: { check_command: "tcp", vars.tcp_port: "3306" }},
              tags: ["add-hosts"] }
```

As you have noticed, there is a new variable in place: **configuration_logic**.

When configuration_logic is "object", the role will generate host definitions following an [Object logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#configuration-best-practice)
 
**TODO**: When configuration_logic variable is "apply", the role will generate host definitions following an [Apply logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#using-apply)

Given that icinga2 and Classic UI are already installed, we are interested only to _add-hosts_ tag. Please take note that icinga2-ansible-add-hosts does not install Icinga2, and must be used after icinga2-ansible-no-ui.

Save the file and close, then launch the playbook with Ansible:

`ansible-playbook site.yml -i inventory_dbservers.ini -t add-hosts`

At this point you should see the newly host entries in `/etc/icinga2/conf.d/hosts`.

Each entry should look like this:

```bash
  # A host definition

object Host "dbserver_one" {
  import "generic-host"
  address = "9.0.1.1"
  
  # Here Goes Vars and check_command

  check_command = "hostalive"
  vars.os = "Linux"
  vars.operator = "on_call"
  vars.sla = "24x7"

}

  #Here Goes Checks

object Service "load_average" {
  host_name = "dbserver_one"
  check_command = "check_nrpe"
  vars.remote_nrpe_command = "check_load"
}

object Service "disk" {
  host_name = "dbserver_one"
  check_command = "check_nrpe"
  vars.remote_nrpe_command = "check_disk"
}

object Service "mysql" {
  host_name = "dbserver_one"
  check_command = "tcp"
  vars.tcp_port = "3306"
}
```
