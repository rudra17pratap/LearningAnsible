# What Ansible is good for?
## `Code driven deployments and operations and complicated set of actions`
  - configuration automation by hand was not a good idea
  - infra provisioning take delays as well
  - on-demand infra in cloud computing
  - ansible use for orchestration performance
    - complicated actions
    - with specific order
  - create new hosts file
  - vim webapp
  ```
  [web]
  host1
  host2
  host3
  
  [lb]
  lb1
  lb2
  
  [all:vars]
  ansible_connection=local
  # to connect locally to all these fake hosts
  ```
  - vim webdeply.yaml
  ```
  ---
  - name: Deploy
    hosts: web
    serial: 1
    # so that each host is executed through all the tasks , before moving to next hosts
    
    tasks:
      # disable the node
      - name: disable node
        debug:
          msg: "disbale {{ inventory_hostname }}"
       # delegate use for redirect connection, below addressing the first node of the lb group
        delegate_to: "{{ groups['lb'][0] }}"
        
       # upgrade the software
       - name: upgrade web
         debug:
           msg: "upgrade software"
           
       # reenable the node
       - name: enable node
         debug:
           msg: "enable {{ inventory_hostname }}"
         delegate_to: "{{ groups['lb'][0] }}"
  ```
  - `ansible-playbook -i webapp webdeply.yaml`
  - so the task are made with only one lb and if serial is not there then it would had skipped, `[host1 -> lb1]`
## 
