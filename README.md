# Primary_Repo
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
![Network Diagram drawio](https://user-images.githubusercontent.com/66979236/172724717-3711381d-d666-4df7-886d-b8c062b60602.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will  conform to availability.In addition to restricting access to the network, load balancers ensures availability of web servers at any instance. 
Essentially, if one of the servers were to go down, then the load balancer directs traffic to the server is operational
The load balancer comprises both front and back end connections.It has a public IP address which is shared by the two DVWA web servers 
In the current network design the DVWA servers are connected to the jump box through SSH protocol established betweeen the ansible docker container in the jump box and the webservers.
Therefore, the jump box mediates connection to the DVW web servers. 
other advantages of the jump box include:
(a) Acting as a  central hub to connect to other potential virtual machines that could be added to the network when required.

(b) Enables connection between the local host and the virtual network 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Usage  and system logs.

A filebeat is used for forwarding and centralizing log data. It is installed as an agent on the servers, filebeat monitors the log files or locations specified by the user, collects log events and forwards them to
either elasticsearch or logstash for indexing. 


A metricbeat is an application installed on our servers that periodically collects metrics from the operating system and  from services running on the server.
Metricbeat takes the metrics and statistics that it collects and ships them to output such as elasticsearch or logstash.
The configuration details of each machine may be found below.

| Name               | Function        | Private IP | Public IP      | Operating System | Docker Containers   | Container Function                       |
|--------------------|-----------------|------------|----------------|------------------|---------------------|------------------------------------------|
| Jump-Box-Provision | Gateway         | 10.0.0.4   | 20.127.29.204  | Linux            | Inspiring_Willamson | Ansible, SSH, Gateway                    |
| Web-1              | Web Server      | 10.0.0.5   | N/A            | Linux            | dvwa                | Host D*mn Vulnerable Web App             |
| Web-2              | Web Server      | 10.0.0.6   | N/A            | Linux            | dvwa                | Host D*mn Vulnerable Web App             |
| ELK-Stack-Server   | Data Aggregator | 10.1.0.4   | 20.114.149.100 | Linux            | elk                 | Host Elastic Search, Logstash and Kibana |

The public IP address of Web-1 and Web-2 is the front end of the load balancer which is the public facing web address.
### Access Policies

The machines on the internal network are not exposed to the public Internet. 
!C:\Users\Home\Downloads\ReadMe Virtual Image.jpg
Only the Host machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
| 65.28.241.134 |


Machines within the network can only be accessed via SSH by the Jump-Box-Provisioner virtual machine.
A summary of the access policies in place can be found in the table below.
| Name                 | Pubicly Accessible | Allowed Source IP Addresses and Ports | Allowed Destinations |
|----------------------|--------------------|---------------------------------------|----------------------|
| Jump-Box-Provisioner | Yes                | 65.28.241.134                         | Virtual Network      |
| Web-1                | No                 | 10.0.0.4:22, 65.28.241.134            | Virtual Network      |
| Web-2                | No                 | 10.0.0.4:22, 65.28.241.134            | Virtual Network      |
| Elk-Stack-Server     | Yes                | 10.0.0.4:22, 65.28.241.134            | Virtual Network      |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
manual configurations can often run into issues where much time is spent configuring software to be used on a particular machine.
Anisble avoids this isue by following a consistent, reliable and simple "playbook", as it is referred to in the application, 
to setup configurations in the same way everytime. In conjunction with docker, a platform as a service software that uses
OS-level virtualization in the form of "containers" to implement software, ansible playbooks can be used to ensure appropriate installation of applications as containers in different VMs.
Ansible playbooks were leveraged in this project to ensure consistent installation of the ELK machine, the D*mn vulnerable web application machine, and the metricbeat and filebeat data collection software.

The playbook implements the following tasks:

(1) Installs Docker service

(2) Installs Python

(3) Installs Docker Module

(4) Increases the VMs virtual memory

(5) Configures the BM to use it's increased memory

(6) Downloads an image of an ELK container and launches it

(7) Ensures that docker is started on boot 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!C:\Users\Home\Downloads\readme virtual image2.jpg

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

(i) 10.0.0.5

(ii)10.0.0.6

We have installed the following Beats on these machines:

(i) Firebeat

(ii) Metricbeat

These Beats allow us to collect the following information from each machine:

(i) Filebeat collects system logs, service logs, authentication logs and also tracks sudo commands.

(ii) Metricbeat collects statistics and metrics of the system.

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned 

SSH into the control node and follow the steps below:
(i) Navigate to the /etc/ansible directory with
	cd /etc/ansible
    
(ii) Create role directory to contain metricbeat and filebeat playbooks.
	mkdir roles
    
(iii) Update the hosts file to include the elk and webserver address.
	
    [webservers]
    
	10.0.0.5 ansible_python_interpreter=/usr/bin/python3
	10.0.0.6 ansible_python_interpreter=/usr/bin/python3
	
    [elk]
    
	10.1.0.4 ansible_python_interpreter=/usr/bin/python3
    
(iv) Copy the Filebeat-config and Metricbeat-config to the /etc/ansible directory.

(v) Copy the Filebeat-Playbook and Metricbeat-Playbook into the /etc/ansible/roles directory.

(vi) From with in the /etc/ansible/roles directory run each playbook with

	ansible-playbook filebeat-playbook.yml
    
	ansible-playbook metricbeat-playbook.yml
    
(vii) Nagivate to http://[your.VM.IP]:5601/app/kibana.Use the public IP address of the ELK server to verify installation procedure

### Verifcation of the ELK stack installation through analyzing kibana logs
To verfiy that the ELK stack is receiving filebeat and metricbeat logs from DVWA servers, an SSH traffic was created in the network of DVWA machines via the jump-box provisioner. 
The following figures display the traffic that is captured by the filebeat logs installed in the machines thus validating the installation procedure. In addition, the figures provide an inkling
on how to interpret SSH logins and identify critical parameters describing the traffic. 


