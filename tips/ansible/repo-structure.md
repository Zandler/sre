# Repository structure

**DISCLAIMER**: some content i **forked** from ansible docs directly. AS IS. for me its better has all thinks in one repo , not in multiples links.



#### **USE DINAMIC INVENTORY**
Do not use static, under no circumstances.
[click here](https://docs.ansible.com/ansible/2.8/user_guide/intro_dynamic_inventory.html)

#### About environments

If you have multiples environments, here some tips:
- **AWS** Use tags like `env:production`
- One file per environment, but defined groups based on propose:
Example:

```bash
# file: production 

[prd_pix_node_groups]
k8s-pix-clt_1.example.com
k8s-pix-clt2.example.com

[prd_db_servers]
db-pix-01.example.com
db-pix-02.example.com

[k8s_finance_node_groups]
k8s-finance-clt_1.example.com
k8s-finance-clt2.example.com

[finance_db_servers]
db-finance-01.example.com
db-finance-02.example.com

# DB Servers all
[database_servers:children]
pix_db_server
finance_db_servers

```

follow this for hml/dev/uat environments


#### **Directory Layout**

The top level of the directory would contain files and directories like so:

```bash
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1.yml             # here we assign variables to particular groups
   group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```

#### Ansible shared libraries and plugins

Shared libraries and plugins are located in the `lib_utils` role.




```bash
# file: roles/common/tasks/main.yml

- name: be sure ntp is installed
  yum:
    name: ntp
    state: present
  tags: ntp

- name: be sure ntp is configured
  template:
    src: ntp.conf.j2
    dest: /etc/ntp.conf
  notify:
    - restart ntpd
  tags: ntp

- name: be sure ntpd is running and enabled
  service:
    name: ntpd
    state: started
    enabled: yes
  tags: ntp
```

Here is an example handlers file. As a review, handlers are only fired when certain tasks report changes, and are run at the end of each play:

```bash
# file: roles/common/handlers/main.yml
- name: restart ntpd
  service:
    name: ntpd
    state: restarted
```

#### What This Organization Enables (Examples)

Above we’ve shared our basic organizational structure.

Now what sort of use cases does this layout enable? Lots! If I want to reconfigure my whole infrastructure, it’s just:

`ansible-playbook -i production site.yml`

To reconfigure NTP on everything:

`ansible-playbook -i production site.yml --tags ntp`

To reconfigure just my webservers:

`ansible-playbook -i production webservers.yml`

For just my webservers in Boston:

`ansible-playbook -i production webservers.yml --limit boston`

For just the first 10, and then the next 10:

`ansible-playbook -i production webservers.yml --limit boston[0:9]
ansible-playbook -i production webservers.yml --limit boston[10:19]`

And of course just basic ad-hoc stuff is also possible:

`ansible boston -i production -m ping
ansible boston -i production -m command -a '/sbin/reboot'`

And there are some useful commands to know:

**confirm what task names would be run if I ran this command and said "just ntp tasks"**

`ansible-playbook -i production webservers.yml --tags ntp --list-tasks`

**confirm what hostnames might be communicated with if I said "limit to boston"**

`ansible-playbook -i production webservers.yml --limit boston --list-hosts`


