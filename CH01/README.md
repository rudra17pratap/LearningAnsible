# What is Ansible ?
## `Introduction`
  - a task execution engine, used to perform one or more actions in one computer engine
  - target local as well as remote computers
  - must be able to work on multiple machines at a same time
  - mode of operation: linear
  - rolling deployments
    - smaller batches in specific order
    - ease a change into production
    - minimize impact of a specific service
  - free strategy: run as fast as can, where actions can be performed on any hosts
  - was created for fund development but was later acquired by Red Hat
  - written in python and interact with [yaml](https://www.yaml.io/) formatted files, eg of [playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html)
  - links
    - [Red Hat Ansible](https://www.ansible.com/products/automation-platform)
    - [github](https://github.com/ansible)
    - [code contributors](https://github.com/ansible/ansible/pulse/monthly)
    - [Ansible Project](https://www.ansible.com/community)
  - Lightweight fleet management system with a contorller and set of fleet
  - communication is done over `ssh`
  - requirements
    - inventory
    - state directives
    - credentials
## `Getting Started`
  - [ansible doc](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - can use built in package manager `dnf`
  - for fedora > 22
    - `sudo dnf install ansible`
    - `which ansible`
    - `ansible --version`
    - `sudo dnf remove ansible`
  - for multiple ver we can have py virtual env
    - `sudo dnf install python-virtualenv`
    - `sudo dnf install gcc openssl-devel`
    - `virtualenv ~/ansible`
    - `source ~/ansible/bin/activate`
    - `pip install ansible`
    
