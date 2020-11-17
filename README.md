# Elk-Stack-Project_Ap
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

/Volumes/STORAGE/GIT_REPO/Elk-Stack-Project_Ap/Diagrams/web-farm-net-diagram_ap.jpg

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the configuration file may be used to install only certain pieces of it, such as Filebeat.

  - /Volumes/STORAGE/GIT_REPO/Elk-Stack-Project_Ap/Ansible/My_first_playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available , in addition to restricting direct access of the web servers from Internet/outside to the network.
- What aspect of security do load balancers protect? the load balancer also protect the network against DDoS attacks.
  What is the advantage of a jump box? the jump-box( gatwaybox on the diagram) is the only server with access to outside network and connection
this configuration enables the system admin to securely connected to the cloud instances (the web servers) protecting these systems from direct exposure to the Internet /hackers attack as they have more port being opened, also the gatwaybox is configure to only accept ssh connection using sshKey and also from one a specific IP from the Internet. as represented in the security_group-rule configuration 


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network  and system files and user permission and security pattern.
- What does Filebeat watch for? filebeat is one off the tool on the family of open-source data shippers that you install as agents on your servers to send operational data to Elasticsearch.Filebeat is designed to ship log files.
- What does Metricbeat record? just like Filebeat metricbeat also is an open-source log shipper that is focused on collection the metric log and sending them to an elasticsearch server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
|Gateway-vm| Gateway  | 10.0.0.6*   | Linux /ubuntu18  | * this node has a also a public IP for management with a firewall rule
| WEB1     |webserver | 10.0.0.4   | Linux /ubuntu18  |
| WEB2     |webserver | 10.0.0.5   | Linux /ubuntu18  |
| ELKvm1   | log-stash| 10.1.0.5   | Linux /ubuntu18  |



### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Gateway( jumpbox) machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Add whitelisted IP addresses (this is the public/Internet IP of the system admin host)

Machines within the network can only be accessed by ssh using an ssh key and from the gateway vm .
- Which machine did you allow to access your ELK VM? What was its IP address? all the web servers have access to the ELK vm so they filebeat and metricbeat can send log over, but we have the Gateway ( jumpbox also being monitored)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.4 10.0.0.5  
                                               10.0.0.5  |
|          |                     |                      |
|          |                     |                      |


Priority|	Name	                   |Port	       |Protocol	|Source	              |Destination	    |Action
300	    |   ssh	                       |22	           |  TCP	    |73.10x.25x.xx	      |10.0.0.6	        |Allow
310	    | Port_80	                   |80	           | Any	    |73.10x.25x.xx	      |VirtualNetwork	|Allow
320	    | Port_8080	                   |560,192,005,044|Any	        |73.10x.25x.xx	      |VirtualNetwork	|Allow
65000	| AllowVnetInBound	           |Any	            |Any        |VirtualNetwork	      |VirtualNetwork	|Allow
65001	| AllowAzureLoadBalancerInBound|	Any	        |Any	    |AzureLoadBalancer	  |   Any	        |Allow
65500	| DenyAllInBound	           | Any	        |Any	    |Any	              |   Any	        | Deny


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the setup is fast automated and reusable. this enables a rapid deployment of the essential component of the ELK subsystems.Using Ansible also eliminate error and misconfiguration.

The playbook implements the following tasks:
- Configure Elk VM with Docker: this is the step the Ansible logs in to the VM and start the configuration or running the plays
- Install docker.io:in this the docker engine is install on the ELK VM 
- Install python3-pip in the play Python is install 
- Install Docker module
- Increase virtual memory , in this play the system parameter are modified so the VM can use more memory 
- download and launch a docker elk container; this is the last play were the Docker images for ELK are pull and run

The following screen-shot displays the result of running `docker ps` after successfully configuring the ELK instance.
/Volumes/STORAGE/GIT_REPO/Elk-Stack-Project_Ap/Diagrams/docker_ps.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- | Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
|Gateway-vm| Gateway  | 10.0.0.6*   | Linux /ubuntu18  |
| WEB1     |webserver | 10.0.0.4   | Linux /ubuntu18  |
| WEB2     |webserver | 10.0.0.5   | Linux /ubuntu18  |



We have installed the following Beats on these machines:
- Check that data is received from the Filebeat system module and the metricbeat module successfully installed and received from this module

These Beats allow us to collect the following information from each machine:
- for the metricbeat I  Collect metrics from the operating system and the container
and from the filebeat I collect system event and changes 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat configuration  file to Ansible node _____.
- Update the the configuration file to file to include the IP address of the ELK server component you want to sent the log to.
- Run the playbook, and navigate to the portal of the ELK servers (web console) to check that the installation worked as expected.

- Which file is the playbook? filebeat configuration file.
- Where do you copy it? https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat  _
- Which file do you update to make Ansible run the playbook on a specific machine? the host file on the Ansible machine in the /etc/ansible dir 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ that is done in the 2 file the filebeatconfig.ymml and the filebeat-install.yml
- Which URL do you navigate to in order to check that the ELK server is running? http://%5Byour.VM.IP%5D:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._


you can the curl tool : curl http://xxxxxxx.net/xxx.yml
