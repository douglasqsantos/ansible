# Ansible Repository 

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

## Executing the Zabbix Agent Role

Let's check the playbook syntax
```bash
ansible-playbook --syntax-check -i inventories/homolog zbx_agent_playbook.yml

playbook: zbx_agent_playbook.yml
```

Let's execute the role
```bash
ansible-playbook -i inventories/homolog zbx_agent_playbook.yml

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