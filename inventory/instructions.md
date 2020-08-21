git clone https://github.com/vilasvarghese/ansible.git

cd ansible/inventory

Introduction to inventory
-------------------------
	1. List inventory based on default setup 
		ansible-inventory --list
		Default inventory files: /etc/ansible/hosts
		
	2. List inventory based on inventory1 file 
		ansible-inventory -i inventory1 --list	
		

	What is the difference you see? Why do you see this difference?

	3. Ping nodes from a specific list
		Execute ping module on all hosts listed in your custom inventory1 file.
		ansible all -i inventory1 -m ping
	

	
Groups 
------
	Inventory file can organized into groups and subgroups of servers. 
	This enables manage group of servers together.
	
	1. Hosts can be grouped and managed together
	2. A host can belong to multiple groups.

	3. Good practice is - group based on
		a. What the hosts are e.g. frontend, backend, db, cache
		b. Why is it required. e.g. primary, secondary, etc
		c. Where are they? e.g. EastUS, WestUS, ect.
		d. When will they be used. e.g. Dev, Prod, Stage etc.
		
	4. Two additional default groups 
		a. all
		b. ungrouped
		
	
	Refer: inventory2
	Example of grouping inventory
		ansible-inventory -i groupinventory --list
	
	
Group of groups
---------------
	metagroup:
		A parent group which has groups under it.
		ansible-inventory -i metagroup --list
		
	Select a  group 
		ansible-inventory -i metagroup --graph web_dev

	Select a  metadata 
		ansible-inventory -i metagroup --graph production

	 
Host Aliases
------------
	Aliases can be defined for nodes
	Helps to understand what that node/host represent.
	Can reference in commands and playbooks.

	e.g of defining alias
	dbserver ansible_host=192.168.176.143
		ansible_host= is mandatory
		IP address or hostname can be used.

	Refer hostalias
	ansible-inventory -i hostalias --host dbhost
	ansible-inventory -i hostalias --host webserver
	ansible-inventory -i hostalias --host ans-server
	
#host may not work without an alias
#More details of ansible can be found https://docs.ansible.com/ansible/2.4/ansible-inventory.html


Host Variables
--------------
	Variables can used in inventory files
	Customize Ansible’s default behavior when connecting and executing commands on your nodes. 
	Accessible from your playbooks
		enables further customization for individual hosts and groups.
	
	a. Alias: an example of using variables.
		ansible_host: where to find the remote nodes
		
	Inventory/Host variables 
		can be set per host or per group. 
		
Refer variableinventory1
	ansible-inventory -i variableinventory1 --host webserver
	ansible-inventory -i variableinventory1 --host dbhost
	ansible-inventory -i variableinventory1 --host ans-server
	

Variables in group - Refer variableinventory2
ansible-inventory -i variableinventory2 --host dbhost
	
For more details on inventory refer: https://docs.ansible.com/ansible/latest/network/getting_started/first_inventory.html#add-variables-to-inventory


group_vars
----------
Ansible uses combination of  
	hosts file and  
	group_vars directory 
to pull variables per host group and run Ansible plays/tasks against hosts.

	group_vars/all
	group_vars/all is used to set variables that will be used for every host that Ansible is ran against.





Using Patterns to Target Execution
----------------------------------