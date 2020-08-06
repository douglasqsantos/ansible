# Ansible Repository

## TODO:
- [X] Upgrade the venv
- [X] Configure the timezone
- [X] Configure docker Ubuntu/Debian
- [X] Configure docker CentOS
- [X] Configure ipv6
- [X] LVM
- [ ] LVM into a Role
- [ ] Cadvisor
- [X] Node_exporter
- [ ] logspout
- [ ] MongoDB

## Installing on Mac OS X

Creating the Virtual Env
```bash
python3 -m venv venv_ansible
```

Activating the Virtual Env
```bash
source venv_ansible/bin/activate
```

Upgrading the pip command line
```bash
pip install --upgrade pip setuptools
```

Installing the Ansible Stable Version
```bash
pip install --upgrade ansible
```

Checking if we have the ansible
```bash
ansible --version
ansible 2.9.11
  config file = /Volumes/Data/git/ansible/ansible.cfg
  configured module search path = ['/Users/douglas/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /Volumes/Data/git/ansible/venv/lib/python3.8/site-packages/ansible
  executable location = /Volumes/Data/git/ansible/venv/bin/ansible
  python version = 3.8.5 (default, Jul 21 2020, 10:48:26) [Clang 11.0.3 (clang-1103.0.32.62)]
```

Testing the installation
```bash
ansible all -i localhost, -c local -m ping
localhost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

## Installing on Ubuntu

Installing the dependencies
```bash
sudo apt install python3-venv python3-pip
```

Creating the Virtual Env
```bash
python3 -m venv venv_ansible
```

Activating the Virtual Env
```bash
source venv_ansible/bin/activate
```

Upgrading the pip command line
```bash
pip3 install --upgrade pip setuptools
```

Installing the Ansible Stable Version
```bash
pip3 install --upgrade ansible
```

Checking if we have the ansible
```bash
ansible --version
ansible 2.9.11
  config file = /Volumes/Data/git/ansible/ansible.cfg
  configured module search path = ['/Users/douglas/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /Volumes/Data/git/ansible/venv/lib/python3.8/site-packages/ansible
  executable location = /Volumes/Data/git/ansible/venv/bin/ansible
  python version = 3.8.5 (default, Jul 21 2020, 10:48:26) [Clang 11.0.3 (clang-1103.0.32.62)]
```

Testing the installation
```bash
ansible all -i localhost, -c local -m ping
localhost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

## Installing on Windows

- Install Docker on the Windows Machine
- Download the Repository
- Build the Image with **docker-compose build**

Building the Image
```
docker build -t ansible .
```

Now we can exec the image
```bash
docker run --rm -it -v "${PWD}:/ansible/workspace" -w /ansible ansible fish
```

Now we need to import the environment
```bash
source venv/bin/activate.fish
```

Checking if we have the ansible
```bash
ansible --version
ansible 2.9.11
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /ansible/venv/lib/python3.8/site-packages/ansible
  executable location = /ansible/venv/bin/ansible
  python version = 3.8.5 (default, Jul 20 2020, 23:11:29) [GCC 9.3.0]
```

## Commands to Use

Ping to localhost
```bash
ansible all -i localhost, -c local -m ping
```

Ping a Group of hosts
```bash
ansible mongo_prod -i inventories/production -m ping
```

Let's check the playbook syntax
```bash
ansible-playbook --syntax-check -i inventories/homolog playbooks.yml
```

Getting information about the SO
```bash
ansible mongo_prod -i inventories/production -m setup -a 'filter=ansible_distribution*'
```

Executing a playbook in a single host
```bash
ansible-playbook -i inventories/homolog playbooks.yml --limit ubuntu01
```

Checking the tags availables
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags
```

Executing only tasks with tag bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --tags="bashrc"
```

Excuting only tasks with tag ssh_pubkeys
```bash
ansible-playbook -i inventories/homolog playbooks.yml --tags="ssh_pubkeys"
```

Checking the tags and filtering the bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags --tags=bashrc
```

Checking the tags and skip the bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags --skip-tags=bashrc
```

Let's execute the Playbook
```bash
ansible-playbook -i inventories/homolog playbooks.yml
```

Now we can execute only the role with the target tag
```bash
ansible-playbook -i inventories/production playbooks-production.yml --tags="role_docker"
```

We can execute the playbook skipping some role tags like
```bash
ansible-playbook -i inventories/production playbooks-production.yml --skip-tags="role_docker"
```

We can execute the playbook for a single host and with a single role tag
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --limit ubuntu01 --tags="role_docker"
```

We can execute the playbook for a single host with a single role tag and setting the variable
```
ansible-playbook -i inventories/homolog playbooks-homolog.yml --limit ubuntu01 --tags="role_docker" --extra-vars "remove_docker='yes'"
```

We can execute the playbook for a group of hosts with a single role tag and setting the variable
```
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_docker" --extra-vars "remove_docker='yes'"
```

Creating a new Module
```bash
ansible-galaxy init monitoring_tools
```

Removing the Node_Exporter
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_monitoring_tools" --extra-vars "remove_node_exporter='yes'"
```

## Role docker

Enabling the Role
```yaml
vim playbooks-homolog.yml
    - role: docker
      vars:
        docker_install: true
      tags:
        - role_docker
```

Install the Docker
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_docker"
```

Removing the Docker from the hosts
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_docker" --extra-vars "remove_docker=true"
```

## Role node_exporter

Enabling the Role
```yaml
vim playbooks-homolog.yml
[...]
    - role: node_exporter
      vars:
        node_exporter_install: true
      tags:
        - role_node_exporter
```

Installing the Node Exporter
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_node_exporter"
```

Removing the Node_Exporter from the hosts
```bash
ansible-playbook -i inventories/homolog playbooks-homolog.yml --tags="role_node_exporter" --extra-vars "remove_node_exporter=true"
```

## Using Modules and Others
- https://docs.ansible.com/ansible/latest/modules/yum_module.html
- https://docs.ansible.com/ansible/latest/modules/apt_module.html
- https://docs.ansible.com/ansible/latest/modules/sysctl_module.html#sysctl-module
- https://docs.ansible.com/ansible/latest/modules/file_module.html#file-module
- https://docs.ansible.com/ansible/latest/modules/wait_for_module.html
- https://docs.ansible.com/ansible/latest/modules/systemd_module.html
- https://docs.ansible.com/ansible/latest/modules/template_module.html
- https://docs.ansible.com/ansible/latest/modules/fetch_module.html
- https://docs.ansible.com/ansible/latest/modules/find_module.html#find-module
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#math
- https://docs.ansible.com/ansible/latest/modules/parted_module.html
- https://docs.ansible.com/ansible/latest/modules/authorized_key_module.html
- https://docs.ansible.com/ansible/latest/modules/filesystem_module.html#filesystem-module
- https://docs.ansible.com/ansible/latest/modules/lvg_module.html
- https://docs.ansible.com/ansible/latest/modules/lvol_module.html
- https://docs.ansible.com/ansible/latest/modules/command_module.html#command-module
- https://docs.ansible.com/ansible/latest/modules/get_url_module.html
- https://docs.ansible.com/ansible/latest/modules/mount_module.html
- https://docs.ansible.com/ansible/latest/modules/synchronize_module.html
- https://docs.ansible.com/ansible/latest/modules/import_tasks_module.html
- https://docs.ansible.com/ansible/latest/modules/import_role_module.html
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html
- https://galaxy.ansible.com/docs/contributing/creating_role.html
- https://docs.ansible.com/ansible/latest/modules/archive_module.html
- https://docs.ansible.com/ansible/latest/modules/unarchive_module.html
- https://docs.ansible.com/ansible/latest/modules/user_module.html
- https://docs.ansible.com/ansible/latest/modules/group_module.html
- https://docs.ansible.com/ansible/latest/modules/copy_module.html


## References
- https://github.com/ansible
- https://github.com/ansible/ansible-examples
- https://www.shellhacks.com/python-install-pip-mac-ubuntu-centos/
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html
- https://docs.ansible.com/ansible/latest/modules/authorized_key_module.html
- https://docs.ansible.com
- https://docs.ansible.com/ansible/latest/index.html
- https://github.com/github/gitignore/tree/master/Global
- https://goo.gl/b68auU
- https://docs.ansible.com/ansible/latest/reference_appendices/config.html
- https://docs.ansible.com/ansible/latest/plugins/connection/ssh.html
- https://docs.ansible.com/ansible/latest/modules/list_of_files_modules.html
- https://docs.ansible.com/ansible/latest/modules/command_module.html#command-module
- https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html
- https://github.com/yaml/pyyaml/wiki/PyYAML-yaml.load(input)-Deprecation
- http://yaml.org/spec/1.2/spec.html
- https://en.wikipedia.org/wiki/YAML
- https://stackoverflow.com/questions/3790454/in-yaml-how-do-i-break-a-string-over-multiple-lines
- https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html
- https://www.middlewareinventory.com/blog/ansible-find-examples/
- https://devops.stackexchange.com/questions/9209/loop-chmod-with-ansible
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html
- https://docs.ansible.com/ansible/latest/modules/find_module.html
- https://www.toptechskills.com/ansible-tutorials-courses/how-to-fix-usr-bin-python-not-found-error-tutorial/
- https://geekflare.com/ansible-ad-hoc-command/
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters_ipaddr.html
- https://www.howtoforge.com/ansible-guide-ad-hoc-command/
- https://sites.google.com/site/mrxpalmeiras/ansible/ansible-cheat-sheet
- https://www.middlewareinventory.com/blog/ansible-get-ip-address/
- https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html
- https://docs.ansible.com/ansible/latest/modules/systemd_module.html
- https://docs.ansible.com/ansible/latest/modules/sysctl_module.html#sysctl-module
- https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings-locations
- https://docs.ansible.com/ansible/latest/modules/wait_for_module.html
- https://blog.zabbix.com/zabbix-agent-active-vs-passive/9207/#:~:text=Active%20checks,-Changing%20the%20active&text=This%20is%20the%20list%20of,minutes%20to%20request%20the%20configuration.&text=In%20the%20same%20zabbix_agentd.,also%20a%20parameter%20called%20Hostname.
- https://stackoverflow.com/questions/32866053/ansible-templates-with-timestamps
- https://linuxconfig.org/redhat-8-configure-ntp-server
- http://www.mydailytutorials.com/working-date-timestamp-ansible/
- https://stackoverflow.com/questions/57102717/how-to-gather-facts-about-disks-using-ansible
- https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#id9
- https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html
- https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_documenting.html#developing-modules-documenting
- https://docs.ansible.com/ansible/latest/modules/authorized_key_module.html
- https://www.percona.com/blog/2020/04/27/how-do-ansible-tags-work/#:~:text=A%20tag%20is%20an%20attribute,execute%20a%20subset%20of%20tasks.&text=Now%20to%20see%20the%20effect,know%20which%20tasks%20will%20run.
- https://linuxconfig.org/redhat-8-configure-ntp-server
- https://www.mydailytutorials.com/working-ansible-variables-conditionals/
- https://docs.ansible.com/ansible/latest/modules/get_url_module.html
- https://www.shellhacks.com/ansible-when-variable-is-defined-exists-empty-true/

