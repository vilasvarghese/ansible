Creating user in ansible

	a. System vs Regular Users
	Linux users can be 
		"system" users 
			user id (UID) below 1000  
			cannot be used to login.
			e.g. jenkins/www-data/apache user. 
			Used for executing some programs 
		"normal"/"regular" users. 
			UID's >=1000 
			Generally used to log into nodes.
		
Refer /etc/login.defs to understand more system vs regular users:

	b. Ability to Login
		Users cannot login (over SSH or otherwise) 
			unless 
				i. they are assigned a shell, 
					usually /bin/bash. 
					This shell is the program run when you login - 
					you interact with the server through this.
				ii. home directory is assigned.
					By convention (and default), 
						this is created at /home/<username>, 
						but can be any location.
						
	c. Groups
		Users have a primary group
			usually the same name as their username. 
		users can be assigned one or more secondary groups.
		
	
	Came across couple of good links that explains this well..
	Please refer to the same for more details.
	
	https://serversforhackers.com/c/create-user-in-ansible
	https://docs.ansible.com/ansible/latest/modules/user_module.html
	http://minimum-viable-automation.com/ansible/use-ansible-create-user-accounts-setup-ssh-keys/