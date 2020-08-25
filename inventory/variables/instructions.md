1. 	


2. Group Variable

	Ansible group_vars
	------------------
	Uses hosts/inventory file and group_vars directory 
		to set variables for host groups 
	Deploying Ansible plays/tasks against each host/group. 
	"File Name" inside group_var == group’s name 
	Special "File Name": all
	accordingly, the variables will be assigned to that host group or all the hosts.


	group_vars are generically used to store basic identifying variables 
	leverage these with other desirable variables by using include_vars and var_files.

	Group variable are mentioned in group_vars folder.
	
	copy dbservers and webservers to /etc/ansible/group_vars folder
	ansible -i inventory -m shell -a 'echo $TERM' all
	
3. 	Host Variables
	--------------
	There's two possibilities to define host vars
		In the inventory file
		In the host_vars folder (following Ansible best practices)
	
	In the inventory file
	----------------------
	Setting slow: true for the host webserver

	- host: webserver
	  hostvars:
		slow: true
	  groups: [group_1, datacenter_1]

	
	