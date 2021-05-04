# Automated ELK Stack Deployment

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](C:\Users\sbalachandran\OneDrive - VMware, Inc\VMwareCorp\Desktop\Cybersecurity HW\Elk.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

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
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_ Loadbalancers can be used to route traffic to servers in its pool based on load according to the rules configured protecting from denial of service attacks. Jumpbox is used as the only server exposed to external networks and can be used for login into other internal servers configured thereby not exposing web or elk servers

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network, as well as watch system metrics, such as CPU usage; attempted SSH logins; sudo escalation failures; etc.
- _TODO: What does Filebeat watch for?_ Used for monitoring system logs
- _TODO: What does Metricbeat record?_ Used for monitoring System metrics

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | DVWA     | 10.0.0.5   | Linux            |
| Web-2    | DVWA     | 10.0.0.6   | Linux            | 
| Web-3    | DVWA     | 10.0.0.7   | Linux            |
| ELK      | Monitoring| 10.1.0.4  | Linux                 |


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

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because of a) Scalability – Easy to deploy multiple servers for HA & DR purposes if needed in future,
b) Extensibility – With few modifications, playbooks could be used to deploy other types of servers , c) Automation – For testing and debugging purposes, it is easier to create an instance and destroy the instance after task is completed.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](C:\Users\sbalachandran\OneDrive - VMware, Inc\VMwareCorp\Desktop\Cybersecurity HW\dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to ansible control node
- Update the playbook file to include the hosts for which deployment is to be done
- Run the playbook, and navigate to appropriate targets to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
