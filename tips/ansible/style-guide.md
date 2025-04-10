== Ansible

**DISCLAIMER**: some content i **forked** from ansible docs directly. AS IS. for me its better has all thinks in one repo , not in multiples links.

### Name Conventions

- Like python yse pep8 name convention
- Roles MUST be named like `technology_component[_subcomponent]`
Example:
`eks_node_group`
`rds_postgres`

- Variables:

|WHERE|PREFIX|DESCRIPTION|EXAMPLE|
|----|----|----|----|
|cli | **cli_**| When use cli to execute wiht -e | `ansible-playbook -e cli_lang=go play.yml`| 
|global | **g_** |  If a variable are defined outside of ansible roles | `g_env:production`|
|Role | **First letter of all words** | start prefix with first letter | `eks_update_node_group` == ` eung_version:2 ` 
- Ansible Roles SHOULD be named like technology_component[_subcomponent]. example: `rds_postgres` or `sns_subcribe`
