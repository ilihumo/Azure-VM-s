AZURE VIRTUAL MACHINE PROJECT

The files in this repository were used to configure the network depicted below.
Azure Elk Diagram.drawio.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

 - name: filebeat-playbook.yml 
- name: installing and launch filebeat
  hosts: webservers
  become: yes
  tasks:
  - name: download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
  - name: install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb
  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
  - name: enable and configure module
    command: filebeat modules enable system
  - name: setup filebeat
    command: filebeat setup
  - name: start filebeat service
    command: service filebeat start
  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
-name: metricbeat-playbook.yml
- name: installing and launching metricbeat
  hosts: webservers
  become: yes
  tasks:
  - name: download metricbeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb
  - name: install metricbeat deb
    command: dpkg -i metricbeat-7.4.0-amd64.deb
  - name: drop in metricbeat.yml
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml
  - name: enable and configure system module
    command: metricbeat modules enable system
  - name: setup metiricbeat
    command: metricbeat setup
  - name: start metricbeat service
    command: service metricbeat start
  - name: enable service metricbeat on boot
    systemd:
      name: metricbeat
      enabled: yes
---

- Description of the Topology  (diagram)
- Access Policies
- ELK Configuration
  - Beats in Use: filebeat, metricbeat
  - Machines Being Monitored: Elk, Jump-box
- How to Use the Ansible Build:
---

Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly monitored and protected, in addition to restricting access to the network.

What aspect of security do load balancers protect? Balancing the load.
 What is the advantage of a jump box? Layer of security.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the configuration files and system logs.
- What does Filebeat watch for? Allows monitoring of files.
- What does Metricbeat record? Metric is statistical data and monitor data.

The configuration details of each machine may be found below.


| Name       | Function             | IP Address | Operating System |
|---------------|-----------------------|----------------|--------------------------|
| Jump Box | Gateway            | 10.0.0.4      |  Linux  |
| Web-1      | Web Server        | 10.0.0.5      |  Linux  |
| Web-2      | Web Server        | 10.0.0.6      |  Linux  |
| Elk            |Security Monito   | 10.1.0.4      |  Linux  |
---

Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the virtual machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 10.0.0.1
- whitelisted IP addresses: 24.251.108.20

Machines within the network can only be accessed by jump-box.
- Which machines were allowed to access the ELK VM? Web-1 and Web-2.
- What are the IP addresses? 10.0.0.5 and 10.0.0.6.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible   | Allowed IP Addresses |
|----------------|-----------------------------|-------------------------------|
| Jump Box  |                 Yes            |   10.0.0.4      |
| Web-1        |                No              |   10.0.0.5      |
| Web-2        |                No              |   10.0.0.6      |

Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible? Ansible helps create everything all at once. 

The playbook implements the following tasks:

Install Docker.
Downloaded deb and installed it.
Configured Elk.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.




Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List of IP addresses of machines being monitored
10.0.0.5
10.0.0.6
10.1.0.4

- Beats that were successfully installed: Filebeat and Metricbeat.

These Beats allowed to collect the following information from each machine:
Filebeat collects data from files, within the D*nm application.
Metricbeat collects statistical data and monitors data, within the D*nm application.

Using the Playbook
Ansible control node already configured with control node provisioned.

SSH into the control node
Copied the configuration files to files directory from root@271d900a3e10:/etc/ansible#.
Updated the preconfigured file to include the correct IP address.
Run the playbook, and navigate to ansible-playbook filebeat-playbook.yml 


- Which file is the playbook? Filebeat-playbook.yml metricbeat-playbook.yml
Where do you copy it? In the directory called files.
*- Which file do you update to make Ansible run the playbook on a specific machine? 
*How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
*- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
