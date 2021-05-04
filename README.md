# Automated ELK Stack Deployment

This document contains the following details:
•	Description of the Topology
•	DVWA Configuration
•	ELK Configuration 
o	Beats in Use
o	Machines Being Monitored
•	How to Use the Ansible Build
•	Access Policies
Description of the Topology
This repository includes code defining the infrastructure below.
 
 
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the "D*mn Vulnerable Web Application"
Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network. The load balancer ensures that work to process incoming traffic will be shared by both vulnerable web servers. Access controls will ensure that only authorized users — namely, ourselves — will be able to connect in the first place.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network, as well as watch system metrics, such as CPU usage; attempted SSH logins; sudo escalation failures; etc.
The configuration details of each machine may be found below with their respective private IP address
Name	Function	Private IP Address	Operating System
Jump Box 	Gateway	10.0.0.4	Linux
Web-1	Web Server	10.0.0.5	Linux
Web-2	Web Server	10.0.0.6	Linux
Web-3	Web Server	10.0.0.7	Linux
ELK 	Monitoring	10.1.0.4	Linux
In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box.
DVWA Configuration
The Web VM exposes an DVWA instance. Docker is used to download and manage the container. Rather than configure DVWA VM manually, we opted to develop a reusable Ansible Playbook to accomplish the task. This playbook is duplicated below.
To use this playbook, one must log into the Jump Box, then issue: ansible-playbook pentest.yml. This runs the pentest.yml playbook on the Web-1, Web-2 and Web-3 host.
ELK Server Configuration
The ELK VM exposes an Elastic Stack instance. Docker is used to download and manage an ELK container.
Rather than configure ELK manually, we opted to develop a reusable Ansible Playbook to accomplish the task. This playbook is duplicated below.
To use this playbook, one must log into the Jump Box, then issue: ansible-playbook install_elk.yml elk. This runs the install_elk.yml playbook on the elk host.


Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the IP address 40.76.116.68
Machines within the network can only be accessed by each other. The Web-1,Web-2 and Web-3 VMs send traffic to the ELK server.
A summary of the access policies in place can be found in the table below.
Name	Publicly Accessible	Allowed IP Addresses
Jump Box	Yes	40.76.116.68
ELK	No	10.1.0.1-254
Web-1	No	10.0.0.1-254
Web-2	No	10.0.0.1-254
Web-3	No	10.0.0.1-254

Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually which helps with the following
1)	Scalability – Easy to deploy multiple servers for HA & DR purposes if needed in future
2)	Extensibility – With few modifications, playbooks could be used to deploy other types of servers
3)	Automation – For testing and debugging purposes, it is easier to create an instance and destroy the instance after task is completed.
The playbook implements the following tasks:
•	Navigate to hosts file in ansible to enter the ELK server IP
nano /etc/ansible/hosts   
•	Navigate to ansible configuration file to confirm the remote user is accurate
Nano /etc/ansible/ansible.cfg 
•	The header of the Ansible playbook can specify a different group of machines as well as a different remote user 
 
•	Before you can run the elk container, we need to increase the memory as this is a system requirement for the ELK containe
 
•	The playbook should then install the following services: 
o	docker.io
o	python3-pip
o	docker, which is the Docker Python pip module.
 
•	After Docker is installed, download and run the sebp/elk:761 container.The container should be started with these published ports: 
o	5601:5601
o	9200:9200
o	5044:5044
 

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
  
The playbook is duplicated below.
---
# install_elk.yml
- name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: elk
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
Target Machines & Beats
This ELK server is configured to monitor Web-1, Web-2 and Web-3 VMs, at 10.0.0.5,10.0.0.6 and 10.0.0.7 respectively.
We have installed the following Beats on these machines:
•	Filebeat
•	Metricbeat
•	Packetbeat
These Beats allow us to collect the following information from each machine:
•	Filebeat: Filebeat detects changes to the filesystem. Specifically, we use it to collect Apache logs.
•	Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.
•	Packetbeat: Packetbeat collects packets that pass through the NIC, similar to Wireshark. We use it to generate a trace of all activity that takes place on the network, in case later forensic analysis should be warranted.
The playbook below installs Filebeat on the target hosts. The playbook for installing Metricbeat is not included but looks essentially identical — simply replace filebeat with metricbeat, and it will work as expected.
---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
Using the Playbooks
In order to use the playbooks, you will need to have an Ansible control node already configured. We use the jump box for this purpose.
To use the playbooks, we must perform the following steps:
•	Copy the playbooks to the Ansible Control Node
•	Run each playbook on the appropriate targets
The easiest way to copy the playbooks is to use Git:
$ cd /etc/ansible
$ mkdir files
# Clone Repository 
$ git clone https://github.com/yourusername/project-1.git
# Move Playbooks and hosts file Into `/etc/ansible`
$ cp project-1/playbooks/* .
$ cp project-1/files/* ./files
This copies the playbook files to the correct place.
Next, you must create a hosts file to specify which VMs to run each playbook on. Run the commands below:
$ cd /etc/ansible
$ cat > hosts <<EOF
[webservers]
10.0.0.5
10.0.0.6
10.0.0.7

[elk]
10.1.0.4
EOF
After this, the commands below run the playbook:
$ cd /etc/ansible
$ ansible-playbook install_elk.yml 
$ ansible-playbook install_filebeat.yml 
$ ansible-playbook install_metricbeat.yml
To verify success, wait five minutes to give ELK time to start up.
Then, run: curl http://10.1.0.4:5601. This is the address of Kibana. If the installation succeeded, this command should print HTML to the console.

