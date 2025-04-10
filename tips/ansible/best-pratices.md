// vim: ft=asciidoc

**DISCLAIMER**: some content i **forked** from ansible docs directly. AS IS. for me its better has all thinks in one repo , not in multiples links.

# Ansible Best Practices Guide





#### Required 

- **GIT** Always use git. 
- Always use yaml files, always with .yml not .yaml (all ansible docs use .yml)
- Except in static inventory. They are ini files.
- Always use yaml files, always with .yml not .yaml (all ansible docs use .yml)
- Except in static inventory. They are ini files.
- Custom Ansible modules SHOULD be embedded in a role.
- your modules has more than 3 parameters? keep in yaml dictionary
- This rule above has the same in parameters
- Use command module, only if necessary use shell module (security risk when use shell baceusa  `- shell: /bin/ls {{ cli_list_parameter }}` has so much power)
- Check all variables in playbook first.
```yaml
# Example
- hosts: acme.example.com
  gather_facts: no
  tasks:
  - fail: msg="g_environment must be set and non empty"
    when: g_environment is not defined or g_environment == ''
```
- Ansible tasks SHOULD NOT be used in Ansible playbooks. Instead, use pre_tasks and post_tasks. An Ansible play is defined as a Yaml dictionary. Because of that, Ansible doesn't know if the play's tasks list or roles list was specified first. Therefore Ansible always runs tasks after roles.
- All tasks in a role SHOULD be tagged with the role name. This is because you can skip or run a role based on tag with `--skip-tags` or `--tags`. More useful when you has multiples roles but need to test one. 

- Upgrade / Update YUM/DNF/APT with ansible package module. Example:
```yaml
- name: Install etcd (for etcdctl)
  dnf: name=etcd state=latest
  when: ansible_pkg_mgr == dnf
  register: install_result
```
- If modules has state paramter , use it. This meke clear you intention.
- Group by roles. Target based on role. A system can be multiples groups.
- Usse group_by module when you have diferent paramater between two diferent operation system. This creates a certain criteria even this not defined in inventory. Example:

```yaml
---

 - name: talk to all hosts just so we can learn about them
   hosts: all
   tasks:
     - name: Classify hosts depending on their OS distribution
       group_by:
         key: os_{{ ansible_facts['distribution'] }}

 # now just on the CentOS hosts...

 - hosts: os_CentOS
   gather_facts: False
   tasks:
     - # tasks that only happen on CentOS go here
```
This will throw all system into dynamic group based on OS

- Always name tasks, provide description about whysomething is being done. 

- **KISS(Keep it simple stupid)**
starts with a simple idea, make simple things, do not use everything of ansible. 
if you finish, ask someone to read and explain what this make. if they hard to understand, probaly is, may you need to splify things.