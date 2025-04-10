# PLAYBOOK ROLE AND TASK

#### Playbook and roles 

create a file with name main.yml that defines your product. Import others playbooks from that:

```yaml
---
# file: main.yml
- import_playbook: k8s_clusters.yml
- import_playbook: db_servers.yml
```
inside k8s)cluters.yml map clusters group to the roles:

```yaml
# file: k8s_clusters.yml 
- hosts: k8s_clusters
  roles:
    - upgrade
    - sidecar
```

it`s more easy if you need limit execution:

`
ansible-playbook main.yml --limit k8s_clusters
`
#### Tasks 
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
