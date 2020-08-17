## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted in the following path:

Images/ELK_Project.drawio

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Kibana YAML playbook files file may be used to install only certain pieces of it, such as Filebeat or Metricbeat. The full file can be found at Scripts/install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, otherwise known as Damn Vulnerable Web Application.

It is important to note that the deployment contains a load balancer. Load balancing ensures that the application will be highly available within the system, yet restricts access to the network and prevents overloads to the machines. In addition, the deployment sports a jumpbox with an SSH key to restrict access to the system only to the person who controls the local workstation, which protects the system from unauthorized access.

The deployment contains an ELK server allows users to easily monitor the vulnerable VMs for changes to the logfiles and system metrics with the use of Filebeat and Metricbeat. Metricbeat records a collection of hardware metrics while Filebeat records a collection of logs created by the system's applications or services. The combination of these two collections will ensure that the VMs are thorouhgly monitored.

The configuration details of each machine may be found below.

| Name                 | Function        | IP Address | Operating System    |
|----------------------|-----------------|------------|---------------------|
| Jump Box Provisioner | Gateway         | 10.0.0.4   | Linux(Ubuntu 18.04) |
| Web-1                | DVWA Container  | 10.0.0.5   | Linux(Ubuntu 18.04) |
| Web-2                | DVWA Container  | 10.0.0.6   | Linux(Ubuntu 18.04) |
| ELK-VM               | ELK Server Host | 10.1.0.4   | Linux(Ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-73.115.119.12
 It is important to note that this IP address can only be changed through Microsoft Azure's NSG rules. 

Machines within the network can only be accessed by the Jumpbox provisioner, which has a public dynamic IP address but a singular private IP address.
-10.0.0.4. 
This can only be accessed through the local workstation IP address, as stated above. To improve security, access to the jumpbox machine has been secured with a private SSH key,which only the local workstation user has.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible   | Allowed IP Addresses |
|--------------|-----------------------|----------------------|
| Jump Box     | Only to a specific IP | 73.115.119.12        |
| Web-1        | No                    | 10.0.0.4             |
| Web-2        | No                    | 10.0.0.4             |
| Kibana Server| Only to a specific IP | 73.115.119.12        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually. The purpose of using Ansible was to enable the jumpbox virtual machine to deploy and manage ansible containers throughout multiple Virtual Machines in a much easier fashion. Instead of having to manually configure multiple VMs, the ansible installation playbook will deploy ansible containers automatically on as many VM's as necessary.

The playbook implements the following tasks:
- Increases the Virtual Machine's memory utilizing sysctl.
- Utilizes the apt service to install docker.io assets.
- Utilizes the apt service to install python's pip3, as well as the python docker module.
- Downloads the elk docker container from the web image.
**Note: It is important to note the published ports for the container download, since it will allow the kibana server to get their respective information through these ports.**
- Enables the docker service.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 Machine, with an IP address of 10.0.0.5
- Web 2 Machine, with an IP address of 10.0.0.6
- ELK Machine, with an IP address of 10.1.0.4

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

As stated previously, these Beats allow us to collect the following information from each machine:
Filebeats: Monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch.
Metricbeat: Monitors the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch. 


### Using the Playbooks
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to your control node.
- Update the hosts file to include the private IP addresses of the VM you want to deploy the ELK. The HOSTS files will have groups that one can add new machines under, such as webservers or elk.
- Run the playbook, and access the Kibana Server through the machine's public IP address. The format would be "http://[your.VM.IP]:5601/app/kibana".
To run the playbooks, utilize the command "ansible-playbook path/install-elk.yml".

### "X"beats Playbooks
The Scripts/file_and_metric_beat_installation.yml file contains the installation measures required for the kibana server to pick up filebeats and metricbeats. As stated previously, this file can be edited so that the Kibana server receives other types of files, in which installation rules can be found within the Kibana repository. 
