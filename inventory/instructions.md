git clone https://github.com/vilasvarghese/ansible.git

cd ansible/inventory

Introduction to inventory
-------------------------
	1. List inventory based on default setup 
		ansible-inventory --list
		#find default inventory executing 
			ansible --version 
			check what is the inventory configured in the configuration file mentioned (e.g. ansible.cfg)
		Default inventory files: /etc/ansible/hosts
		repeat this command after adding inventory with parameters


		To list host
			$ ansible groupname -i hosts --list-hosts
		
	2. List inventory based on inventory1 file 
		ansible-inventory -i inventory1 --list	
		

	What is the difference you see? Why do you see this difference?

	3. Ping nodes from a specific list
		Execute ping module on all hosts listed in your custom inventory1 file.
		ansible all -i inventory1 -m ping
		ansible centos -i 1ansibleuser.yaml - m ping
		
		Ping a specific host
		ansible <host alias> -m ping
		ansible cent1 -m ping


Inventory can take options like
	
Using users 
-----------
Modify 1ansibleuser.yaml
ansible all -i 1ansibleuser.yaml -m ping

Using users with private key
----------------------------
Modify 1ansibleuser.yaml
ansible all -i 2ansibleuserkey.yaml -m ping




	
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
		d. When will they be used. e.g. Debug, upgrade, monitor
		e. How are you planning to use it e.g. e.g. Dev, Prod, Stage etc.
		f. Who will use it e.g. SRE, dev, projecx
		
For e.g.		
----------------------------------------------------------------------------------		
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    test:
      hosts:
        bar.example.com:
        three.example.com:		
----------------------------------------------------------------------------------		
one.example.com exists in the 	
	dbservers, 
	east, and 
	prod 


	4. Two additional default groups 
		a. all
		b. ungrouped
		
		
		
		
	
	Refer: inventory2
	Example of grouping inventory
		ansible-inventory -i groupinventory --list
		ansible all -i 4groupinventory -m ping

Group of groups
---------------
	metagroup:
		A parent group which has groups under it.
		ansible-inventory -i metagroup --list
		
	Select a  group 
		ansible-inventory -i metagroup --graph web_dev

	Select a  metadata 
		ansible-inventory -i metagroup --graph production
		ansible-inventory -i metagroup --graph development

	 
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

N.B: In metagroups you cannot mention host aliases

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


Instruction for folder assignment
---------------------------------
	create a folder called group_vars
	create a file with group name
	give the properties in key: value format
	#ansible-inventory --list

host_vars
----------
	we can also define variables per host in host_vars folder
	create a folder called host_vars
	create a file with host name
	give the properties in key: value format
	#ansible-inventory --list
	

---------------------------------------------------
	variables can be injected into groups using 
		group_name:vars in inventory	
		group_name folder
	
	cd variables 
	ansible-playbook -i group-vars group-vars-test.yml
-------------------------------------------------------	




Using Patterns to Target Execution
----------------------------------
https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html
ansible <pattern> -m <module_name> -a "<module options>"
	e.g. ansible webservers -m service -a "name=httpd state=restarted"
	
	

	All hosts	
		all (or *)

	One host	
		hostname

	Multiple hosts	
		host1:host2 (or host1,host2)

	One group	
		webservers(group_name)

	Multiple groups	(all hosts in webservers plus all hosts in dbservers)
		webservers:dbservers



	Excluding groups(all hosts in webservers except those in atlanta)
		webservers:!atlanta

	Intersection of groups	(any hosts in webservers that are also in staging)
		webservers:&staging

	Combine Patterns. This example:
		webservers:dbservers:&staging:!phoenix

	wildcard patterns with FQDNs or IP addresses
		192.0.\*
		\*.example.com
		\*.com
		
	Limitations of patterns
		IP/Hostname should be present in inventory.
		
	
		
For more advanced options refer https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html
	
	
Default location for inventory
	/etc/ansible/hosts

Once an inventory is defined
	use patterns to select 
		hosts or 
		groups 

specify a different inventory file 
	-i <path> option. 

Type of inventory files
	Static 
	Dynamic
	
	server could be both a 
		dbserver and
		webserver.

	hosts that run on a non-standard SSH port
		put the port number after the hostname with the colon. 
	The Ports listed in the SSH configuration file that can be used with the OpenSSH connection but not use with the paramiko connection.

	To makes things explicit, it is suggested that you set them if items are not running on the default ports:
		badwolf.example.com:5309  

Suppose you have static IPs and want to set up some aliases that live in your host file
	you can connect through tunnels. 
	Also, you can describe the hosts like the below example:
		Jumper ansible_port=5555 ansible_host=192.0.2.50  






	Group Variables
		The variables can be applied to an entire group at once, such as:
			

	Hosts Variables
		You can assign the variables to the hosts that will be used in playbooks, such as:
			host1 http_port=80 maxRequestsPerChild=808  
			
				maxRequestsPerChild can be a variable in the playbook now.
				
				
ranges of hosts				
----------------
[webservers]
www[01:50].example.com

or in yml
---
  webservers:
    hosts:
      www[01:50].example.com:


Ranges are inclusive. You can also define alphabetic ranges
	01 - 50
	
You can also define alphabetic ranges
	[databases]
	db-[a:f].example.com


increments between sequence numbers are permitted
[webservers]
www[01:50:2].example.com
	01 - 50 with increment of 2



Assigning a variable to many machines: group variables
--------------------------------------------------------------------

[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com

atlanta:vars: is the way to say variables for the group atlanta


In yml
atlanta:
  hosts:
    host1:
    host2:
  vars:
    ntp_server: ntp.atlanta.example.com
    proxy: proxy.atlanta.example.com
	
	
	
	
Inheriting variable values: group variables for groups of groups	
----------------------------------------------------------------

[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2

[usa:children]
southeast
northeast
southwest
northwest



Child groups have a couple of properties to note:
	Any host that is member of a child group is automatically a member of the parent group.
	A child group’s variables will have higher precedence (override) a parent group’s variables.
	Groups can have multiple parents and children, but not circular relationships.
	Hosts can also be in multiple groups, but there will only be one instance of a host, merging the data from the multiple groups


	Valid file extensions include 
		‘.yml’, 
		‘.yaml’, 
		‘.json’, or 
		no file extension

	Ansible loads host and group variable files by searching paths relative to 
		the inventory file or 
		the playbook file. 
	If your inventory file at /etc/ansible/hosts contains a host named ‘foosball’ that belongs to two groups, 
		‘raleigh’ and 
		‘webservers’, that host will use variables in YAML files at the following locations:
		/etc/ansible/group_vars/raleigh # can optionally end in '.yml', '.yaml', or '.json'
		/etc/ansible/group_vars/webservers
		/etc/ansible/host_vars/foosball
	



----------------------------------
----------------------------------
----------------------------------
----------------------------------
----------------------------------

Additional Good reference: https://www.golinuxcloud.com/ansible-variables/

Tips and tricks: https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

if you have different user than root for root user, then it can be updated in the 
	ansible.cfg file