## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/khot0002/Virtual-Network-Project/MyElkStackNetowrk.drawio

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

---
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:

  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: drop in metricbeat config
    copy:
     src: /etc/ansible/files/metricbeat.yml
     dest: /etc/metricbeat/metricbeat.yml

  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

  - name: setup metric beat
    command: metricbeat setup

  - name: start metric beat
    command: service metricbeat start

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting overload to the network.
- _Load balancers act as a security control that provides the ability to monitor, detect, and respond to cyber threats. Load balancers also serve to increase the capacity and the reliability of the web applications. The advantage of having a jump box is to access and manage devices in a separate secured zone where the server is monitored and controlled from a trusted machine or network, such as security patches, updates to applications, etc. can all be done in one area and applied to all machines.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system metrics.
- Filebeat collects data about the file system. Filebeat logs and monitors data and forwards them to an indexing service like Elasticsearch or Logstash.   
- Metricbeat collects machine metrics from an operating system, such as uptime and other services that are running on the server. The statistics are collected and is stored in indices like Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function          | IP Address | Operating System        |
|----------|-------------------|------------|-------------------------|
| Jump Box | Gateway           | 10.1.0.4   | Linux                   |
| Ansible  | Deploy Containers | 10.1.0.4   | Unix-like               |
| DVWA 1   | Web Containers    | 10.1.0.5   | OS-level virtualization |
| DVMA 2   | Web Container     | 10.1.0.7   | OS-level virtualization |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 13.90.45.137

Machines within the network can only be accessed by jump box.
- Two virtual machines were allowed access to the ELK VM--VM1 (10.1.0.5) and VM2 (10.1.0.7)

A summary of the access policies in place can be found in the table below.

| Name                       | Publicly Accessible | Allowed IP Address |
|----------------------------|---------------------|--------------------|
| Jump Box                   | Yes/No              | 73.65.86.161       |
| SSH from Jump Box          | No                  | 10.1.0.4           |
| Allow Elk Ports Web to Elk | No                  | 10.1.0.5, 10.1.0.7 |
| Allow Elk                  | No                  | 73.65.86.161       |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the automated configuration with Ansible, which is able to deploy and operate different stages of the development lifecycle. Ansible only requires installation on the master server and communicates to other nodes via SSH. There is also very low uptime, meaning there is no need to install other clients to get the network up and running. 

The playbook implements the following tasks:
- Set up a new virtual machine that will run as the ELK server.
- Configure a new Ansible playbook to use for your new ELK virtual machine.
- Playbook will run commands to download and install images of Elasticsearch, Logstash, and Kibana.
- Depending on the VM, run the following command to increase memory: "sysctl -w vm.max_map_count=262144"
- After Docker is installed, download and run the sebp/elk container.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!https://drive.google.com/file/d/1Ma4APl8hd8X5M0E0VXPbyafRypxwCrbK/view?usp=sharing

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.5
- 10.1.0.7

We have installed the following Beats on these machines:
- Filebeats and Metricbeats were both successfully installed on both virtual machines: VM1 and VM2

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._Filebeat collects data about the file system. Filebeat logs and monitors data and forwards them to an indexing service like Elasticsearch or Logstash. Metricbeat collects machine metrics from an operating system, such as uptime and other services that are running on the server. The statistics are collected such as cpu/mem/disk/network metrics, and can also monitor Apache, Docker, Nginx, Redis, etc.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to /etc/ansible.
- Update the configuration file to include the IP addresses and servers. 
- Run the playbook, and navigate to the website to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- The playbook file is listed as /ansible.cfg and you copy it to the /etc/ansible directory.
- The file that needs to be updated is filebeat-configuration.yml and you will need to specify the IP addresses for the VMs.
- Navigate to http://[your.VM.IP]:5601 in order to verify that the ELK server is running properly.



