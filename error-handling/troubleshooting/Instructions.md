Reference:
	https://docs.ansible.com/ansible-tower/2.2.0/html/administration/troubleshooting.html
	https://www.redhat.com/sysadmin/troubleshoot-ansible-playbooks
	
Problems connecting to your host
	Can you ssh to your host? 
	Are your hostnames and IPs correctly added in your inventory file? 
	
	
Problems running a playbook
---------------------------
	1. Are you authenticating with the user currently running the commands? 
		If not, 
			check how the username 
			or pass the --user=username or -u username or --ansible_user.
	2. Is your YAML file correctly indented? 
		You may need to line up your whitespace correctly. 
		Indentation level is significant in YAML. 
		You can use yamlint to check your playbook. 
		Refer: http://docs.ansible.com/YAMLSyntax.html
	3. Items beginning with a - are considered list items or plays. 
		Items with the format of key: value 
			operate as hashes or 
			dictionaries. 
		Ensure you don’t have extra or missing - plays.
	
	
	4. Which versions and paths of the Ansible binaries and Python are you running?
		If you added OS packages or Python modules required by your playbooks, 
		Is the Ansible interpreter "seeing" them?
		From the command line, run 
			ansible --version 
		check the python version and is ansible identifying the right python.


View a listing of all ansible_ variables
------------------------------------------------------
	Ansible by default gathers “facts” from 
		machines it manages, 
		accessible in Playbooks 
			and in templates. 
	To view all facts available about a machine, run the setup module as an ad-hoc action:

	ansible -m setup hostname
	This prints out a dictionary of all facts available for that particular host.

Locate and configure the configuration file
	Ansible 
		does not require a configuration file
		OS packages include a default one in /etc/ansible/ansible.cfg 
			for possible customization. You 
			can also install your own copy in ~/.ansible.cfg or 
			keep a copy in a directory relative to your playbook named as ansible.cfg.

	refer ansible.cfg to understand properties to be adde.


Running in verbose mode
---------------------------

From the command line, 
	add -v : Simple verbose
	(or 
		-vv, 
		-vvv, 
		-vvvv, 
		-vvvvv: Most verbose. 
	
	highest verbosity levels can sometimes be too much information
	better increase gradually in multiple executions until you get what you need.

Level 4 
	troubleshoot connectivity issues 
level 5 is useful for WinRM problems.

	From Tower
		select the VERBOSITY level from the job template definition.
		Note: Remember to disable the debug mode 
			after you resolve 
			the situation because the detailed information is only useful for troubleshooting.

in debug mode, 
	the values of certain variables (passwords, for example) 
		will be displayed unless no_log option in the task, so erase the outputs when you are done.

More options refer 
	https://www.redhat.com/sysadmin/troubleshoot-ansible-playbooks


Playbook Stays in Pending
---------------------------
	Run ansible-tower-service restart on the Tower server.
	Check to ensure that the /var/ partition has more than 1GB of space available. Jobs will not complete with insufficient space on the /var/ partition.
	Ensure all supervisor services are running via supervisorctl status.
	Check to see if disabling PRoot solves your issues (and, if so, please report back to Ansible’s Support team to let them know it helped). Navigate to the /etc/tower/settings.py file and set AWX_PROOT_ENABLED=False, then restart services with the ansible-tower-service-restart command.
	If you continue to have problems, run sosreport as root on the Tower server, then file a support request with the result (contact Ansible via the Red Hat Customer Portal at https://access.redhat.com/).




Running in verbose mode
---------------------------







Tower server errors 
	logged in /var/log/tower. 
Supervisors logs 
	in /var/log/supervisor/. 


 Cancel a Tower Job
When issuing a cancel request on a currently running Tower job, Tower issues a SIGINT to the ansible-playbook process. While this does cause Ansible to exit, Ansible is designed to finish tasks before it exits and only does so after the currently running play has completed.

With respect to software dependencies, if a running job is canceled, the job is essentially removed but the dependencies will remain.


Problems when running a job
---------------------------
	If you are having trouble running a job from a playbook, 
		you should review the playbook YAML file. 
	When importing a playbook, 
		manually or 
		via a source control 
		keep in mind that the host definition is controlled by Tower and should be set to hosts: all.
