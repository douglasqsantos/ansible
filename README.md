# Ansible Repository 

## TODO:
- [ ] Upgrade the venv
- [ ] Configure the timezone
- [ ] Configure docker
- [ ] Configure ipv6

Upgrade the venv
```bash
python3 -m venv venv_ansible
source bin/venv_ansible/activate
pip install --upgrade pip setuptools
```

## Installing on Mac OS X

Installing the VirtualEnv
```bash
sudo pip3 install virtualenv pyyaml
```

Creating the Virtual Env
```bash
virtualenv venv
```

Activating the Virtual Env
```bash
source venv/bin/activate
```

Upgrading the pip command line
```bash
pip install --upgrade pip
```

Installing the Ansible Stable Version
```bash
pip3 install ansible
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
sudo apt install python3-pip fish -y
```

Installing the VirtualEnv
```bash
sudo pip3 install virtualenv pyyaml
```

Creating the Virtual Env
```bash
virtualenv venv
```

Activating the Virtual Env
```bash
source venv/bin/activate
```

Upgrading the pip command line
```bash
pip install --upgrade pip
```

Installing the Ansible Stable Version
```bash
pip install ansible
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

Now we can exec the image
```bash
docker run --rm -it -v "${PWD}:/ansible/workdir" -w /ansible douglasqsantos/ansible fish
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

Let's access the directory with the source code
```bash
cd workdir/
```

Now let's ping the localhost
```bash
ansible all -i localhost, -c local -m ping
[WARNING]: Ansible is being run in a world writable directory (/ansible/workdir), ignoring it as an ansible.cfg source. For more information see
https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir
[WARNING]: Platform linux on host localhost is using the discovered Python interpreter at /usr/bin/python3, but future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
localhost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## SSH Copy-ID

Before start the playbooks need to copy the ssh key for all nodes as follows
```bash
ssh-copy-id douglas@10.0.0.22
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/douglas/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
douglas@10.0.0.22's password:

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'douglas@10.0.0.22'"
and check to make sure that only the key(s) you wanted were added.
```

## Checking if all the nodes are reacheable 

```bash
ansible linux -i inventories/homolog -m ping
[WARNING]: Platform linux on host debian01 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
debian01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
centos01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
ubuntu01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Listing the information about the SO
```bash
ansible linux -i inventories/homolog -m setup -a 'filter=ansible_distribution*'
[WARNING]: Platform linux on host debian01 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
debian01 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Debian",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "10",
        "ansible_distribution_release": "buster",
        "ansible_distribution_version": "10",
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false
}
ubuntu01 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "18",
        "ansible_distribution_release": "bionic",
        "ansible_distribution_version": "18.04",
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false
}
centos01 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "CentOS",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/redhat-release",
        "ansible_distribution_file_variety": "RedHat",
        "ansible_distribution_major_version": "8",
        "ansible_distribution_release": "Core",
        "ansible_distribution_version": "8.2",
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false
}
```

## Executing into a single host

```bash
ansible-playbook -i inventories/homolog playbooks.yml --limit ubuntu01
```

## Executing tasks with tags

Checking the tags availables
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags

playbook: playbooks.yml

  play #1 (linux): linux	TAGS: []
      TASK TAGS: [bashrc, bashrc_common, bashrc_root, motd, ssh_pubkeys, vimrc, vimrc_common, vimrc_root]
```

Checking the tags and filtering the bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags --tags=bashrc

playbook: playbooks.yml

  play #1 (linux): linux	TAGS: []
      TASK TAGS: [bashrc, bashrc_common, bashrc_root]
```

Checking the tags and skip the bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags --skip-tags=bashrc

playbook: playbooks.yml

  play #1 (linux): linux	TAGS: []
      TASK TAGS: [motd, ssh_pubkeys, vimrc, vimrc_common, vimrc_root]
```

Executing only tasks with tag bashrc
```bash
ansible-playbook -i inventories/homolog playbooks.yml --tags="bashrc"

PLAY [linux] ***********************************************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************************************
[WARNING]: Platform linux on host debian01 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [debian01]
ok: [ubuntu01]
ok: [centos01]

TASK [common : Configure the .bashrc for root] *************************************************************************************************************************************************************
ok: [debian01]
ok: [ubuntu01]
ok: [centos01]

TASK [Configure the .bashrc for common user] ***************************************************************************************************************************************************************
ok: [debian01] => (item=douglas)
ok: [ubuntu01] => (item=douglas)
ok: [centos01] => (item=douglas)

PLAY RECAP *************************************************************************************************************************************************************************************************
centos01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Excuting only tasks with tag ssh_pubkeys
```bash
ansible-playbook -i inventories/homolog playbooks.yml --tags="ssh_pubkeys"

PLAY [linux] ***********************************************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************************************
[WARNING]: Platform linux on host debian01 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [debian01]
ok: [ubuntu01]
ok: [centos01]

TASK [common : Set up Authorized Keys] *********************************************************************************************************************************************************************
ok: [debian01] => (item=['douglas', 'id_rsa.pub'])
ok: [centos01] => (item=['douglas', 'id_rsa.pub'])
ok: [ubuntu01] => (item=['douglas', 'id_rsa.pub'])

PLAY RECAP *************************************************************************************************************************************************************************************************
centos01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```


## Executing the Zabbix Agent Role

Checking the tags availables
```bash
ansible-playbook -i inventories/homolog playbooks.yml --list-tags

playbook: playbooks.yml

  play #1 (linux): linux	TAGS: []
      TASK TAGS: [zbx_bkp_files, zbx_config_cli, zbx_delete_defaults, zbx_enable_cli, zbx_install_cli, zbx_install_cli_on_centos, zbx_install_cli_on_debian_base, zbx_install_release, zbx_install_release_on_centos, zbx_install_release_on_debian_base, zbx_remove, zbx_remove_cli, zbx_remove_from_centos, zbx_remove_from_debian_base]
```

Let's check the playbook syntax
```bash
ansible-playbook --syntax-check -i inventories/homolog playbooks.yml

playbook: playbooks.yml
```

Let's execute the role
```bash
ansible-playbook -i inventories/homolog playbooks.yml

PLAY [linux] **************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [ubuntu01]
ok: [centos01]

TASK [zabbix_agent : Remove Packages on CentOS] ***************************************************************************************************************
skipping: [ubuntu01]
changed: [centos01]

TASK [zabbix_agent : Remove Packages on Ubuntu] ***************************************************************************************************************
skipping: [centos01]
changed: [ubuntu01]

TASK [zabbix_agent : Clean useless packages from the cache] ***************************************************************************************************
skipping: [centos01]
changed: [ubuntu01]

TASK [zabbix_agent : Install Zabbix Release on CentOS] ********************************************************************************************************
skipping: [ubuntu01]
changed: [centos01]

TASK [zabbix_agent : Install Zabbix Release on Ubuntu] ********************************************************************************************************
skipping: [centos01]
changed: [ubuntu01]

TASK [zabbix_agent : Install Zabbix Agent on Ubuntu] **********************************************************************************************************
skipping: [centos01]
changed: [ubuntu01]

TASK [zabbix_agent : Install Zabbix Agent on CentOS] **********************************************************************************************************
skipping: [ubuntu01]
changed: [centos01]

TASK [zabbix_agent : Enable Zabbix Agent] *********************************************************************************************************************
ok: [ubuntu01]
changed: [centos01]

TASK [zabbix_agent : Find Zabbix Agent Files] *****************************************************************************************************************
ok: [ubuntu01]
ok: [centos01]

TASK [zabbix_agent : Backup Find Zabbix Agent Files] **********************************************************************************************************
changed: [ubuntu01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 13936, 'inode': 529305, 'dev': 2050, 'nlink': 1, 'atime': 1596141450.1493006, 'mtime': 1593414000.0, 'ctime': 1596141449.1733446, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})
changed: [centos01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf.rpmsave', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 441, 'inode': 67497769, 'dev': 64768, 'nlink': 1, 'atime': 1596141335.7901366, 'mtime': 1596032844.747316, 'ctime': 1596141388.2013996, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})
changed: [centos01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 13936, 'inode': 668866, 'dev': 64768, 'nlink': 1, 'atime': 1593431950.0, 'mtime': 1593431950.0, 'ctime': 1596141466.0330148, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})

TASK [zabbix_agent : Remove Custom files for Zabbix Agent] ****************************************************************************************************
changed: [ubuntu01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 13936, 'inode': 529305, 'dev': 2050, 'nlink': 1, 'atime': 1596141450.1493006, 'mtime': 1593414000.0, 'ctime': 1596141449.1733446, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})
changed: [centos01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf.rpmsave', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 441, 'inode': 67497769, 'dev': 64768, 'nlink': 1, 'atime': 1596141335.7901366, 'mtime': 1596032844.747316, 'ctime': 1596141388.2013996, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})
changed: [centos01] => (item={'path': '/etc/zabbix/zabbix_agentd.conf', 'mode': '0644', 'isdir': False, 'ischr': False, 'isblk': False, 'isreg': True, 'isfifo': False, 'islnk': False, 'issock': False, 'uid': 0, 'gid': 0, 'size': 13936, 'inode': 668866, 'dev': 64768, 'nlink': 1, 'atime': 1593431950.0, 'mtime': 1593431950.0, 'ctime': 1596141466.0330148, 'gr_name': 'root', 'pw_name': 'root', 'wusr': True, 'rusr': True, 'xusr': False, 'wgrp': False, 'rgrp': True, 'xgrp': False, 'woth': False, 'roth': True, 'xoth': False, 'isuid': False, 'isgid': False})

TASK [zabbix_agent : Creating the Zabbix Agent Custom File Directory] *****************************************************************************************
ok: [ubuntu01]
ok: [centos01]

TASK [Template zabbix_agentd.conf] ****************************************************************************************************************************
changed: [ubuntu01]
changed: [centos01]

RUNNING HANDLER [zabbix_agent : Stop Zabbix Agent] ************************************************************************************************************
changed: [ubuntu01]
ok: [centos01]

RUNNING HANDLER [zabbix_agent : Restart Zabbix Agent] *********************************************************************************************************
changed: [centos01]
changed: [ubuntu01]

PLAY RECAP ****************************************************************************************************************************************************
centos01                   : ok=12   changed=8    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
ubuntu01                   : ok=13   changed=9    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
``` 




## Using Modules
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
