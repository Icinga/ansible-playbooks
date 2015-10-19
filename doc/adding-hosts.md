## Adding Host Definitions on Icinga 2 with Ansible roles

After Icinga2 is installed (either headless or alongside a WEB UI), you can move forward to add host definitions.

Given that this role will iterate over all hosts in all groups currently in the playbook scope, you are encouraged to split your monitored hosts in multiple inventories.

### Example: Monitoring Web Servers

What about if you want to monitor a group of web servers?

Create a new inventory file (or use an existing one), with your monitoring server entry in place, and begin to add the web servers to be monitored:

```ini
[monitoring_servers]
icinga2server ansible_ssh_host=1.2.3.4
icinga2server_one ansible_ssh_host=1.2.3.5
icinga2server_three ansible_ssh_host=1.2.3.6

[webservers]
webserver_one ansible_ssh_host=9.0.0.1
webserver_two ansible_ssh_host=9.0.0.2
webserver_three ansible_ssh_host=9.0.0.3
webserver_four ansible_ssh_host=9.0.0.4
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

   - { role: icinga2-ansible-add-hosts,
              configuration_logic: "object",
              host_attributes:
              { vars: { vars.sla: "24x7", vars.operator: "on_call" },
                check_command: { check_command: "http" }},
        
              host_checks:
              { load_average: { check_command: "check_nrpe", vars.remote_nrpe_command: "check_load" },
                disk: { check_command: "check_nrpe", vars.remote_nrpe_command: "check_disk" },
                http: { check_command: "http", vars.http_vhost: "{{ hostvars[item]['ansible_domain'] }}" }},

              tags: ["add-hosts"] }
```

As you have noticed, there is a new variable in place: **configuration_logic**.

When configuration_logic is "object", the role will generate host definitions following an [Object logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#configuration-best-practice)
 
**TODO**: When configuration_logic variable is "apply", the role will generate host definitions following an [Apply logic style](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/monitoring-basics#using-apply)

Given that icinga2 and Classic UI are already installed, we are interested only to _add-hosts_ tag. Please take note that icinga2-ansible-add-hosts does not install Icinga2, and must be used after icinga2-ansible-no-ui.

Save the file and close, then launch the playbook with Ansible:

`ansible-playbook site.yml -i inventory_webservers.ini -t add-hosts`

At this point you should see the newly host entries in `/etc/icinga2/conf.d/hosts`.

Each entry should look like this:

```bash
  # A host definition

object Host "webserver_one.tld" {
  import "generic-host"
  address = "9.0.0.1"
  vars.os = "Linux"
  vars.os_family = "Debian"

  # Here Goes Vars and check_command
  check_command = "http"
  vars.operator = "on_call"
  vars.sla = "24x7"

}

  #Here Goes Checks
object Service "load_average" {
  host_name = "webserver_one.tld"
  check_command = "check_nrpe"
  vars.remote_nrpe_command = "check_load"
}

object Service "disk" {
  host_name = "webserver_one.tld"
  check_command = "check_nrpe"
  vars.remote_nrpe_command = "check_disk"
}

object Service "http" {
  host_name = "webserver_one.tld"
  check_command = "http"
  vars.http_vhost = "domain.tld"
}
```

The variable **http_vhost** is a custom attribute that can be assigned to http check command. See [Plugin Check Commands](http://docs.icinga.org/icinga2/latest/doc/module/icinga2/toc#!/icinga2/latest/doc/module/icinga2/chapter/configuring-icinga2#plugin-check-commands) for a list of all the supported check custom attributes. What's interesting in this example is that **vars.http_vhost** will be automatically populated via `{{ hostvars[item]['ansible_domain'] }}`.
