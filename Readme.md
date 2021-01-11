Objective

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
How this purpose is achieved

Topology

1This topology consists of 2 virtual networks, A & B.

2 Virtual networks, A & B are connected with each other with peering.
• Peering is where two or more networks are connected with each other.
• Peering of virtual networks allows resources in each network to communicate and transfer data across connected networks.
• This peering connection of virtual networks is beneficial because it is seamless, with low latency, and with high bandwidth.

3 In network A, there are following resources:
• A firewall with configured inbound and outbound policies as a first line of defense.
• Behind the firewall, there is a jump box, as a bastion host facing the internet.
• There are 2 web servers hosting DVWA behind the jump box.
• The two web servers are also behind a load balancer, and the load balancer faces the internet.
• The load balancers here prevent the servers from attacks like DDOS, and also enhance redundancy for high availability. (CIA, Model- Availability is ensured).
• Jump box adds an extra layer for the intruders that they have to penetrate in order to access the web servers, thus reducing the likelihood of accessing the webservers. ( As per CIA model, here
we are ensuring keeping data in webservers Confidential, and to maintain the integrity of files)

4 In network B, there are following resources:
• A fire wall facing internet, with configured inbound and outbound policies.
• A virtual Machine (ELK VM)
• Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file logs, and to the system metrics.

Access Policies
5 The machines on the internal network are not exposed to the public Internet.
• Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:

Public IP address of my internet 98.xxx.xxx.xxx
• Machines with in the network including web and ELK servers can only be accessed through the Jump box.
ELK Configuration

6 Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because:
• Multiple machines can be configured and deployed simultaneously which saves time and effort.
• Also results in accurate and consistent configuration across machines, reducing human error.
• Playbook prepared with yml script used to deploy machines performed the following tasks:

Install Docker.io
Installed the image of the container for eg, ELK..
Published open ports
Configured memory settings of the container
Target Machines & Beats

7 This ELK server is configured to monitor the target webservers
• The IP addresses of the target webservers are:

10.0.1.5 & 10.0.1.6
• The following beats were installed on the target servers

File Bea*t; is an agent and a light wait shipper that monitors and forwards log files to ELK servers for visual analytics in Kibana.
Matric Beat; Matric beat performs a similar function of an agent, but collects and reports system/service level matrices. For example CPU,or Memory performance.
Using the Playbook

8 SSH into the control node and follow the steps below:
• Using the following command, download the file beat configuration file on the control node.
• curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
• Make following edits to the above downloaded file:

- Scroll to line #1106 and replace the IP address with the IP address of the ELK machine(40.77.20.19).
- Scroll to line #1806 and replace the IP address with the IP address of the ELK machine(40.77.20.19).
- Save this file in /etc/ansible/files/filebeat-config.yml.

9 Create a new playbook as explained in the steps below:
• Install the .deb file using the dpkg command dpkg -i filebeat-7.4.0-amd64.deb
• Copy the /etc/ansible/files/filebeat-config.yml, to target VM ( web servers).
• The above task is performed using the copy module in the playbook.
• Once the playbook is created to install file beat on target web servers save it with name filebeat-playbook.yml , and run the command:

ansible-playbook filebeat-playbook.yml

• Installing file beat on the target VM using the above command, go to Kibana and verify if the logs are being collected, by typing 40.77.20.19:5601 in the browser

10 Using the same steps download the file for Matric beat, edit the configuration file, and create a new playbook to install it on the target Web Servers.
