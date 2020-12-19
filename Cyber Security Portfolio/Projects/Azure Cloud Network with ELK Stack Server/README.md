## Azure Cloud Network with ELK Stack Server Project

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/mpicione/picione-portfolio/blob/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server/Diagram/Network_Diagram.jpg) 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, individual .yml files may be used to install only certain pieces of it, such as Filebeat.
* [Ansible/web_vm_docker_setup.yml](https://github.com/mpicione/picione-portfolio/blob/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server/Ansible/web_vm_docker_setup.yml)
* [Ansible/elk_server_setup.yml](https://github.com/mpicione/picione-portfolio/blob/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server/Ansible/elk_server_setup.yml)
* [Ansible/filebeat_setup.yml](https://github.com/mpicione/picione-portfolio/blob/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server/Ansible/filebeat_setup.yml)
* [Ansible/metricbeat_setup.yml](https://github.com/mpicione/picione-portfolio/blob/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server/Ansible/metricbeat_setup.yml)

This document contains the following details:
* [Description of the Topology](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#description-of-the-topology)
* [Access Policies](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#access-policies)
* [ELK Configuration](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#elk-configuration) 
  * [Machines Being Monitored](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#machines-being-monitored)
  * [Beats in Use](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#beats-in-use)
* [Using the Playbooks](https://github.com/mpicione/picione-portfolio/tree/main/Cyber%20Security%20Portfolio/Projects/Azure%20Cloud%20Network%20with%20ELK%20Stack%20Server#using-the-playbooks)

### Description of the Topology

Load balancing ensures that the application will be available, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system services.

The configuration details of each machine on the internal network may be found below:

| Name                 | Function       | IP Address | Operating System           |
|----------------------|----------------|------------|----------------------------|
| Jump-Box-Provisioner | Gateway        | 10.0.0.7   | Linux (Ubuntu 18.04-LTS)   |
| Web-01               | Web Server     | 10.0.0.5   | Linux (Ubuntu 18.04-LTS)   |
| Web-02               | Web Server     | 10.0.0.6   | Linux (Ubuntu 18.04-LTS)   |
| Web-03               | Web Server     | 10.0.0.9   | Linux (Ubuntu 18.04-LTS)   |
| ElkServer            | SIEM           | 10.1.0.4   | Linux (Ubuntu 18.04-LTS)   |
| Azure Load Balancer  | Load Balancer  |            |                            |

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump-Box-Provisioner machine and Load Balancer can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
* 137.135.84.226

Machines within the internal network can only be accessed via SSH from the Ansible container installed on the Jump-Box-Provisioner using the IP address listed above.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible? | Allowed IP Addresses           |
|----------------------|----------------------|--------------------------------|
| Jump-Box-Provisioner | Yes                  | [my current public IP address] |
| Web-01               | No                   |                                |
| Web-02               | No                   |                                |
| Web-03               | No                   |                                |
| ElkServer            | No                   |                                |
| Azure Load Balancer  | Yes                  | All HTTP Traffic               |	
		
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it removes human error, saves time and allows administrators to focus on other tasks, and reduces downtime when all or parts of the network must be rebuilt.

The playbooks implement the following tasks:
* Installs Docker
* Installs Python3
* Installs PIP Packages
* Increases Virtual Memory Available to the Server
* Downloads, Configures, and Starts Elk Server Container

#### Machines Being Monitored

This ELK server is configured to monitor the following machines:
* 10.0.0.5
* 10.0.0.6
* 10.0.0.9

#### Beats in Use

We have installed the following Beats on these machines:
* Filebeat
* Metricbeat

These Beats allow us to collect the following information from each machine:
* Filebeat monitors log files and forwards them to the Elk Server for indexing. These can be used to monitor login events, look for unexpected or unusual activity to identify possible malicious attacks on the system.
* Metricbeat monitors statistics from the OS or services running on the server, such as CPU or Memory usage, and forwards them to the Elk Server. These can be used to identify possible hardware issues before they cause outages and detect possible malicious attacks on the system that are using additional system resources.

### Using the Playbooks

In order to use one of the playbooks, you will need to have an Ansible control node already configured with SSH keys already generated and shared appropriately. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:
* Update the /etc/ansible/hosts file to include the local IP address of the machine(s) you are configuring under the appropriate group, [webservers] or [elk]
* Run the playbook, and check that the installation worked as expected.
  * If running web_vm_docker_setup.yml:
	  * Ensure security group rules allow SSH connection from Jump-Box-Provisioner internal IP address
	  * Establish SSH connection from Ansible control node installed on Jump-Box-Provisioner to test if it is functioning correctly
	  * Once connected to the new web_vm run the command "curl localhost/setup.php" to confirm DVWA is running properly
  * If running elk_server_setup.yml:
	  * Ensure security group rules allow SSH connection from Jump-Box-Provisioner internal IP address
	  * Ensure security group rules allow connection to port 5601 from desired IP address
	  * Establish SSH connection from Ansible control node installed on Jump-Box-Provisioner to test if it is functioning correctly
	  * Navigate to https://[ElkServer Public IP]:5601/app/kibana and see if you successfully connect to test if it is functioning correctly
  * If running filebeat_setup.yml:
	  * Navigate to https://[ElkServer Public IP]:5601/app/kibana, click "Add Log Data", click "System Logs", click on "DEB" tab, and click "Check Data" under Module Status to test if it is functioning correctly
  * If running metricbeat_setup.yml:
	  * Navigate to https://[ElkServer Public IP]:5601/app/kibana, click "Add Metric Data", click "Docker Metrics", click on "DEB" tab, and click "Check Data under Module Status to test if it is functioning correctly