
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk](Diagrams/Elk.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat. All the files are located under /etc/ansible

  - install-elk.yml - https://github.com/saib2018/Cybersec2021/blob/main/Ansible/ansible/install-elk.yml
  - filebeat-playbook.yml - https://github.com/saib2018/Cybersec2021/blob/main/Ansible/ansible/filebeat-playbook.yml
  - metricbeat-playbook.yml - https://github.com/saib2018/Cybersec2021/blob/main/Ansible/ansible/metricbeat-playbook.yml 

This document contains the following details:
- Description of the Topology
- DVWA Configuration
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound to the network.
- Loadbalancers can be used to route traffic to servers in its pool based on load according to the rules configured protecting from denial of service attacks. Jumpbox is used as the only server exposed to external networks and can be used for login into other internal servers configured thereby not exposing web or elk servers

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network, as well as watch system metrics, such as CPU usage; attempted SSH logins; sudo escalation failures; etc.
- Filebeat is used for monitoring system logs
- Metricbeat is used for monitoring System metrics

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | DVWA     | 10.0.0.5   | Linux            |
| Web-2    | DVWA     | 10.0.0.6   | Linux            | 
| Web-3    | DVWA     | 10.0.0.7   | Linux            |
| ELK      | Monitoring| 10.1.0.4  | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 40.76.116.68
Machines within the network can only be accessed by each other. The Web-1,Web-2 and Web-3 VMs send traffic to the ELK server


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 40.76.116.68         |
| Web-1    | No                  | 10.0.0.1-254         |
| Web-2    | No                  | 10.0.0.1-254         |
| Web-3    | No                  | 10.0.0.1-254         |
| ELK      | No                  | 10.1.0.1-254         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because of 
- Scalability – Easy to deploy multiple servers for HA & DR purposes if needed in future,
- Extensibility – With few modifications, playbooks could be used to deploy other types of servers 
- Automation – For testing and debugging purposes, it is easier to create an instance and destroy the instance after task is completed.

To deploy the playbook, following steps were performed

 - SSH using remote user into Jumpbox provisioner 40.76.116.68
 - Ran the command "docker container list -a" to identify the containers running
 - Used the command "docker container start {container-name}" and "docker container attach {container-name}" to log into the ansible container
 - Confirmed that ansible host files had the required server IPs listed which were target machines
 ![nano](Diagrams/nano.PNG)
 
 - Confirmed that ansible configuration files had the remote user setup to the admin user of the target machine
 ![Remote_user_config](Diagrams/Remote_user_config.PNG)
 
 - After preparing the playbook file as described below, ran the playbook using the command ansible-playbook filebeat-playbook.yml which is located in /etc/ansible folder

The playbook implements the following tasks:

- The header of the Ansible playbook can specify a different group of machines as well as a different remote user
![header](Diagrams/header.PNG)

- Before you can run the elk container, we need to increase the memory as this is a system requirement for the ELK container
![systemctl](Diagrams/systemctl.PNG)

- The playbook should then install the following services: docker.io, python3-pip and docker, which is the Docker Python pip module
![installmodule](Diagrams/installmodule.PNG)

- After Docker is installed, download and run the sebp/elk:761 container.The container should be started with these published ports: 5601:5601, 9200:9200 and 5044:5044
![port](Diagrams/port.PNG)

- Complete Filebeat-Playbook listed here - https://github.com/saib2018/Cybersec2021/blob/main/Ansible/ansible/filebeat-playbook.yml

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps](Diagrams/dockerps.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.5
- Web-2 10.0.0.6
- Web-3 10.0.0.7

We have installed the following Beats on these machines: 
- Filebeat 
- Metricbeat 
- Packetbeat

These Beats allow us to collect the following information from each machine:
- Filebeat: Filebeat detects changes to the filesystem. Specifically, we use it to collect Apache logs.
- Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.
- Packetbeat: Packetbeat collects packets that pass through the NIC, similar to Wireshark. We use it to generate a trace of all activity that takes place on the network, in case later forensic analysis should be warranted

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to ansible control node
- Update the playbook file to include the hosts for which deployment is to be done
- Run the playbook, and navigate to appropriate targets to check that the installation worked as expected.
- Playbooks are located in the /etc/ansible path and has yml extension.
- Hosts file in ansible contains the hosts for which the playbook would be run against /etc/ansible/hosts
- Navigate to the following URL to confirm if the Kibana is running - http://137.135.24.201:5601/app/kibana (Public IP of the ELK server)
 ![Kibana1](Diagrams/Kibana1.PNG)

- The easiest way to copy the playbooks is to use Git:

```
$ cd /etc/ansible
$ mkdir files
Clone Repository 
$ git clone https://github.com/saib2018/Cybersec2021.git
Move Playbooks and hosts file Into `/etc/ansible`
$ cp Cybersec2021/playbooks/* .
$ cp Cybersec2021/files/* ./files

```
This copies the playbook files to the correct place.
- Next, you must create a hosts file to specify which VMs to run each playbook on. Run the commands below:

```
cd /etc/ansible
cat > hosts <<EOF
[webservers]
10.0.0.5
10.0.0.6
10.0.0.7

[elk]
10.1.0.4
EOF

```

- After this, the commands below run the playbook:

```
cd /etc/ansible
ansible-playbook install_elk.yml 
ansible-playbook filebeat-playbook.yml 
ansible-playbook metricbeat-playbook.yml

```

- To verify success, wait five minutes to give ELK time to start up.
- Then, run: curl http://10.1.0.4:5601. This is the address of Kibana. If the installation succeeded, this command should print HTML to the console.

