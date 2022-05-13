1. 	
Each time Ansible executes a task
	first step is 
		collect facts about nodes. 
	These facts can be access with the hostvars variable. 
	like : hostvars[inventory_hostname]

	ansible-playbook 1readfacts.yml


Ansible retrieves facts about 
	operating system, 
	hardware architecture, 
	memory  
	cpu, and 
	IP addresses. 
	There are interesting use cases to access and use this information, 
		for example you can collect the ip addresses of the hosts and use them to create a Nginx config file.
	
	
	ad-hoc command to get the same from hostvars:
		ansible -i hosts all -m debug -a "var=hostvars[inventory_hostname]"
		ansible localhost -m debug -a "var=hostvars[inventory_hostname]"

	We can store the return value of the command
		save the result of the hostname command in a variable, 
		then print this variable to the console
		
		ansible-playbook 2registerfacts.yml

	


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
		In the inventory file along with host
		In the host_vars folder (following Ansible best practices)

	
	