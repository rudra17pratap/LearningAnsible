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
## Manage system configuration
  - configure the appln and make the service started
  - vim web-app.yaml
  ```
  ---
  - name: configure web app
    hosts: web
    # git source code repo and the ver to checkout
    vars:
      repo: myrepo.com/repo.git
      version: 8
     
    tasks:
      - name: install nginx
        debug:
          msg: "dnf install nginx"
          
      # create a dir to hold the web content
      - name: ensure web dir
        debug: 
          # we can use of file module if the dir is already there
          msg: "/mkdir /webapp"
          
      - name: get content
        debug:
          msg: "git clone --branch {{ version }} {{ repo }} /webapp"
      
      - name: nginx config
        debug:
          msg: "put nginx config  in place"
      
      # check the service is running, use service module
      - name: ensure nginx running
        debug:
          msg: "service nginx start"     
  ```
  - `ansible-playbook -i webapp web-app.yaml`
## React to configuration changes
  - what if we want to change the config of nginx above and then we need to restart it
  - we can use `notify` directive in any task, which instruct to notify handler on change and after notify add `changed_when` to true to reflect the change
  - and we need to define handler section in the play
  - handlers are just special task, with special meaning and only ran when required
  ```
  ---
  - name: configure web app
    hosts: web
    # git source code repo and the ver to checkout
    vars:
      repo: myrepo.com/repo.git
      version: 8
     
    tasks:
      - name: install nginx
        debug:
          msg: "dnf install nginx"
          
      # create a dir to hold the web content
      - name: ensure web dir
        debug: 
          # we can use of file module if the dir is already there
          msg: "/mkdir /webapp"
          
      - name: get content
        debug:
          msg: "git clone --branch {{ version }} {{ repo }} /webapp"
      
      - name: nginx config
        debug:
          msg: "put nginx config  in place"
        notify: restart nginx
        changed_when: True
      
      # check the service is running, use service module
      - name: ensure nginx running
        debug:
          msg: "service nginx start" 
          
    handlers:
      - name: restart nginx
        debug:
          msg: "service nginx restart"
  ```
  - `ansible-playbook -i webapp web-app.yaml -vv`
## Infra Management
  - create new docker containers and play around with those containers
  - vim docker.yaml
  ```
  ---
  - name:
    hosts: 
  ```
