
# Ansible Essentials

## Install ansible
    - sudo apt install ansible

## Create an inventory
    - 
## Run ansible command -i for inventory file and -m is for module pass with ping which is going to access the server with ssh key
    - ansible  all --key-file ~/.ssh/ansible -i inventory -m ping

## Create an ansible config file in the root directory
    - ansible.cfg

## Shorten the command, it will look for the config in the config file which is ansible.cfg 
    - ansible all -m ping
    - ansible all --list-hosts  #it will list all the host
    - ansible all -m gather_facts 
    - ansible all -m gather_facts --limit $IP_ADDRESS #it will return specific information from the specific host/ip address you provided

## Adhoc commands with elevated priviledges
    # -a for argument for the module --become elevate the priveledge --ask-become-pass this will ask for the password
    # this command will do apt update to all your servers with sudo priveledges
    - ansible all -m apt -a update_cache=true --become --ask-become-pass 
    BECOME password: <your_user_password>

    # install vim-nox to all the servers in the inventory list
    - ansible all -m apt -a name=vim-nox --become --ask-become-pass
    BECOME password: <your_user_password>

    # do some update with latest
    - ansible all -m apt -a "name=snapd state=latest" -become --ask-become-pass
    BECOME password: <your_user_password>

    # upgrade distribution all the servers in the inventory list
    - ansible all -m apt -a "upgrade=dist" -become --ask-become-pass
    BECOME password: <your_user_password>

## Create playbook `install_apached.yml` file
    # Run command to run with your playbook
    - ansible-playbook --ask-become-pass install_apache.yml
    BECOME password: *****

    # Remove apache
    - ansible-playbook --ask-become-pass remove_apache.yml
    BECOME password: *****

## The 'when' Conditional
    # Before starting anything do a `git pull` to get all the changes that might your teammates pushed
    - git pull

    # Add another server with different distribution like CentOs
    # In the playbook add `when`
    - when: ansible_distribution == "Ubuntu"
    
    # Gather information to your newly added server
    - ansible all -m gather-facts --limit $NEW_SERVER_IP | grep ansible_distribution
    # For CentOS distribution you need to start the httpd service and enable port 80 in order to work
    - sudo systemctl start httpd
    - sudo firewall-cmd --add-port=80/tcp

## Targeting specific nodes
    # Create a copy of your current `install_apache.yml` playbook and name it `site.yml`
    # Organized your inventory server with specific group and each purpose like web server, db server or file server
    # Run the `site.yml` playbook
    - ansible-playbook ask-become-pass site.yml

## Tags
    # Add tags in the playboook and target specific tags
    # Run the playbook with tags
    - ansible-playbook --tags "apache,db" --ask-become-pass site.yml

## Managing Files
    # Create a `files` directory
    # Add default_site.html file in the files directory

## Managing Services
    # Add new play in site.yml playbook in the web server to start httpd service to start automatically

## Adding Users & Bootstrapping
    # Adding user in the playbook

## Roles
    # Organizing playbook base on each role
    
