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