# Elk_Project
Project 1 - Elk Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Elk_Project/Day_3_Diagram

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - Enter the playbook file
---
 - name: installing and launching filebeat
   hosts: webservers
   become: yes
   tasks:

   - name: download filebeat deb
     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

   - name: install filebeat deb
     command: dpkg -i filebeat-7.4.0-amd64.deb

   - name: drop in filebeat.yml
     copy:
       src: ./filebeat.yml
       dest: /etc/filebeat/filebeat.yml

   - name: enable and configure system module
     command: filebeat modules enable system

   - name: setup filebeat
     command: filebeat setup

   - name: start filebeat service
     command: service filebeat start

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting unauthorized or internet access to the network.
- What aspect of security do load balancers protect? Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider. 
What is the advantage of a jump box? The jump box allows the administrator to only expose one server to the internet and administrate many servers centrally from the jump box. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the application and system configuration.
- What does Filebeat watch for? Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- What does Metricbeat record? Metricbeat collects and reports various system-level metrics for various systems and platforms. Metricbeat also supports internal modules for collecting statistics from specific platforms.

The configuration details of each machine may be found below.

| Name       | Function            | IP Address    | Operating System     |
|------------|---------------------|---------------|----------------------|
| Jump Box   | Gateway             | 23.99.68.105  | Linux (ubuntu 18.04) |
| Elk Server | Data Logs           | 10.1.0.4      | Linux (ubuntu 18.04  |
| DVWA vm1   | Container           | 10.0.0.5      | Linux (ubuntu 18.04) |
| DVWA vm2   | Redundant container | 10.0.0.6      | Linux (ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 104.171.88.184

Machines within the network can only be accessed by the jump server.
- Which machine did you allow to access your ELK VM? The jump server. What was its IP address? 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible | Allowed IP Addresses  |
|----------------|---------------------|---------------------- |
| Jump Box       | Yes                 | 104.171.88.184        |
| DVWA-vm1       | No                  | 10.0.0.4              |
| DVWA-vm2       | No                  | 10.0.0.4              |
| Elk Server     | No                  | 10.0.0.4              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible? The main advantage is Ansible can be run from the command line without the use of configuration files for simple tasks, such as making sure a service is running, or to trigger updates or reboots.

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Make sure you are in your Ansible container and create a new playbook: touch /etc/ansible/install-elk.yml
- Run the following command through the shell: sysctl -w vm.max_map_count=262144
- Install the following packages: docker.io python-pip docker
- Download the Docker container called sebp/elk
- Configure the container to start with the following port mappings: 5601:5601 9200:9200 5044:5044 and start the container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Elk_Project/Day_3_image_Elk_server_sudo_docker_ps

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring: 10.0.0.5 and 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat.

These Beats allow us to collect the following information from each machine:
- Filebeat is used for collecting and shipping log files. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the public key file to ~/.ssh/id_rsa.pub file.
- Update the /etc/ansible/hosts file to include the IP addresses or hostnames of the managed nodes
- Run the playbook, and navigate to managed nodes to check that the installation worked as expected.

Answer the following questions to fill in the blanks:
- Which file is the playbook? The YML file. Where do you copy it?  /etc/ansible/
- Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts How do I specify which machine to install the ELK server on versus which to install Filebeat on? - You put group names in brackets in the /etc/ansible/hosts file to specify where to install
- _Which URL do you navigate to in order to check that the ELK server is running? - http://localhost:9200/_status

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
