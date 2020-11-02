# Parts of Ansible
## `Hosts and Variables`
  - Inventory = set of potential target hosts to execute tasks on
  - collection of hosts
    - simple hosts file
    - groups of hosts file
    - subgroups hosts file
      - [east:country]
        hello
  - support multiple inventory as well
  - with inventory comes the inventory variables
  - used for
    - task arguments
    - templates config
    - ansible behavior
  - create a [dynamic](https://docs.ansible.com/ansible/2.5/dev_guide/developing_inventory.html) inventory sources
## `Tasks`
  - expressed in yaml
  - task data: where db is created and folder is created
  - task controls: looping over the tasks, conditions
  - if multiple same task is executed one by one, so it has captured ret type so that same task won't repeat
  - Modules
    - samll piece of codes + common code + task arguments i/p and then transfered to target host and then get backs the return data
## `Playbooks`
  - collection of one or more plays
  - plays are one or more tasks
  - create inventory
  - vim hosts
  ```
  [groupA]
  host1
  host2
  host3
  
  [groupB]
  host4
  host5
  host6
  
  [all:vars]
  ansible_connection=local
  # allow all the hosts to interacts with them locally
  ```
  - vim demoplays.yaml
  ```
  # plays required a name, host pattern and some tasks (which are expressed as yaml list)
  ---
  - name: "Do a Demo"
    hosts: groupA
    tasks:
      # every task requires a name
      - name: demo task 1
      # every tasks requires a module, here we are using debug module which prints msg on the screen
        debug:
          msg: "this is task 1"
          
      - name: demo task 2
        debug:
          msg: "this is task 2"
          
  # adding a new play
  - name: "Do another Demo"
    hosts: groupB
    tasks:
      - name: demo task 3
        debug:
          msg: "this is task 3"
          
      - name: demo task 4
        debug:
          msg: "this is task 4"
  ```
  - plays and tasks are executed from top to bottom
  - ansible-playbook cmd requires the inventory file and yaml file
  - `ansible-playbook -i hosts demoplays.yaml`
## Control task and play behavior
  - execution abort when play encounter an error
  - so we can replace any `debug` module with a `fail` module to fail any set of play and stop the execution
  - so we can add conditions `when` to skip certain tasks
  ```
  ---
  - name: "Do a Demo"
    hosts: groupA
    tasks:
      - name: demo task 1
        debug:
          msg: "this is task 1"
          
      - name: demo task 2
        fail:
          msg: "this is task 2"
        # only evaluate for hosts2 is true and the boolean condition is checked and skipped
        when: inventory_hostname == "host2"
          
  - name: "Do another Demo"
    hosts: groupB
    tasks:
      - name: demo task 3
        debug:
          msg: "this is task 3"
          
      - name: demo task 4
        debug:
  ```
  - we can have `serial` execution strategy
  ```
  ---
  - name: "Do a Demo"
    hosts: groupA
    tasks:
      - name: demo task 1
        debug:
          msg: "this is task 1"
          
      - name: demo task 2
        fail:
          msg: "this is task 2"
        when: inventory_hostname == "host2"
          
  - name: "Do another Demo"
    hosts: groupB
    # adding serial strategy with batch size 1
    serial: 1
    tasks:
      - name: demo task 3
        debug:
          msg: "this is task 3"
          
      - name: demo task 4
        debug:
  ```
  - so in serial what will happen is , like host 3 will complete both task 3 and task 4 and then hand over to next host 4
  - but in the above set of play, the starting three host, host1 ,2, 3 will execute first demo task 1 and then move onto demo task 2
## Challenge
  - Write a playbook having following req:
    - single play, single task, pip module, ansible python package, virt env
  - ans
    - look for the documentation of [pip](https://docs.ansible.com/ansible/2.4/pip_module.html)
    ```
    ---
    - name: Challenge Me
      hosts: host1
      
      tasks:
        - name: ansible pip
          pip:
          # package name
            name: ansible
          # adding a virtual env value
            virtualenv: ~/challenge1
    ```
   - `ansible-playbook -i hosts challenge1.yaml`
