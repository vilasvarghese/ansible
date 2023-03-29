--------------------------------------------------------
---
- hosts: webservers
  roles:
    - common
    - webservers
--------------------------------------------------------	
If 
	roles/
		/x
			/tasks/main.yml exists, tasks listed will be added to the play.
			/handlers/main.yml exists, handlers listed will be added to the play.
			/main.yml exists, variables listed therein will be added to the play.
			/defaults/main.yml exists, variables listed therein will be added to the play.
			/meta/main.yml exists, any role dependencies listed therein will be added to the list of roles 

	Any 
		copy, 
		script, 
		template or 
		include tasks (in the role) 
		can reference files in roles/x/{files,templates,tasks}/ (dir depends on task) 
	without having to path them relatively or absolutely.
	

Creating a Role
---------------
	Create an Ansible role?
		Be prepared to follow directory structure. 
		

	cd ~/ansible
	mkdir roles
	cd roles
	mkdir apache
	cd apache

	mkdir tasks handlers files templates vars defaults meta 
	
	cd tasks
	
	Compare the file between 
		
		1. Tasks are moved out to tasks folder
		ansible\roles\apache\tasks\main.yaml 
			and
		ansible\playbook\usecases\apache\apache.yaml
			
			 tasks file 
				contains only steps that will be performed when we use the Apache role.

				We need src=index.html and src=vhost.tpl - We will get to those latter..
			/
		2. Handlers are moved to handlers folder
		
			ansible\roles\apache\handlers
				and
			ansible\playbook\usecases\apache\apache.yaml
			
			The handler section is very clear now.
			
		3. tasks and handlers are ready. We need 
			index.html file and 
			vhost.tpl template 
			These are files referenced in tasks/main.yml
			Ansible should find and move them to managed nodes. 
			
			
			Add 
				files/index.html and 
				templates/vhost.tpl 
			
		
------------------------------------------------------------------------------------------------------------------
		Below are not required.
		
			meta folder
		1. If mydependency role has to be executed before executing current-apache role.
			then add meta/main.yaml
			
		
Vars directory
--------------
Preferably use for big projects.
Makes sense to user vars directory when a variable has to be used across directories in roles.

For small projects it's discouraged for.
	Better mention configuration details outside of the role folder.
	Share the role without 
	Safe not exposing sensitive information. 
	variables declared within a role 
		are overridden by variables in other locations. 
	much better to place variable data in playbooks 
		that are used for specific tasks.

“vars” directory is useful for complex roles. 
	e.g. role which needs to support different Linux distributions, 
		specifying default values for variables can be useful to 
			handle different package 
				names, 
				versions, and 
				configurations.

Including other files
---------------------
Sometimes when you create roles with lots of 
	tasks, 
	dependencies, or 
	conditional logic, 
	they will become large and difficult to understand. 

	In situations like this you can split tasks out into their own files 
	
	For e.g. for additional set of tasks to configure TLS for Apache server, 
		separate those out into their own file. 
		We could call the file tasks/tls.yml 

	Include the same in main.yaml as below.
---------------------------------------------------------------------------	
<<rest of the yaml>>
tasks:
- include: roles/apache/tasks/tls.yml
---------------------------------------------------------------------------


Create a Skeleton Playbook
--------------------------

To use roles this way 
	allows us to use playbooks to 
	declare what a server is supposed to do 
	without having to always repeat creating tasks to make it so.


	cd ..
	Refer playbook.yaml


	This file can be very simple. 
	hosts: 
		mention the targetted managed nodes.
	become: true
		gives sudo privileges
	roles:
		list of roles to execute.
		

Refer multiple_roles_playbook.yaml




Ansible Galaxy
--------------
	searchable repository of Ansible Roles.
	user contributed roles 
	ready to add them to playbooks 
	
	https://galaxy.ansible.com/home
	
e.g., 
	mod_security2 can be added to configure Apache 
		adv: extra security settings. 
	We will use an Ansible Galaxy role called apache_modsecurity. To use this role, we’ll download it locally and then include it in our playbook.


	1. Search for all roles that has description "PHP for ...."
		ansible-galaxy search "PHP for RedHat/CentOS/Fedora/Debian/Ubuntu"

		To quit : q
		
		Ansible uses less command when the result count is more.
			paginate by pressing space.

	2. See the details go to 
		https://galaxy.ansible.com/home
		

	3. To install geerlingguy.php for our playbook.
		To download a role for use in our playbook, we use the ansible-galaxy install command:
			ansible-galaxy install geerlingguy.php
	
------------------------------------------------------------------------------------------------------------------