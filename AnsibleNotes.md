DevOps Course Contents


References: 
https://docs.ansible.com/ansible/latest/installation_guide/index.html
C:\PraiseTheLord\HSBGInfotech\DevOps\ansible\

Prerequisits
------------
1. Basic understanding of linux/unix.
	Ansible controller works only with linux/unix
2. Basic knowledge on cloud or vmware.


Training Topics

########################################################################################
•	Ansible – Configuration Management: 12 hours
########################################################################################

	System Engineer
		Lot of repetitive tasks
			Sizing
			Provisioning
			Creating VM's
				Assign Config
				Patching applications
				Deploying applications
				migrating apps
				security compliance audits
		Execute 100's of commands on 100's of different servers
			In certain orders
			with some reboots
		
		Most of them create scripts for these.

	What is ansible?
	----------------
		- Simple automation language 
			perfectly describe and enable an IT application infrastructure 
		- Ansible is powerful IT automation tool  
		- Simple enough for everyone in your IT team to learn and use, 
		- Powerful to meet enterprise class deployment complexity 
			Complex things can be automated
		- Handles repetitive tasks.
				
		
	Some Use cases
		- Configuration management
		  App deployment
		  Provisioning
		  Continous Delivery
		  Security and Compliance
		  Orchestration

	for e.g. 
		Restart few m/c in certain orders
		Powerdown web servers
		Powerdown db 
		Start db
		Start web servers


	Ansible Tower by Red Hat 
		Enterprise framework for 
				controlling 
				securing and 
				managing 
			Ansible automation with UI and restful API.


	Advantages of Ansible
	---------------------
		Simple
			- Human readable
			- No coding skills required
			- Tasks executed in Order
			- Get productive quickly
		Powerful
			- App deployment
			- Configuration Management
			- Workflow orchestration
			- Orchestrate the app lifecycle
		Agentless
			- Agentless architecture
			- Uses OpenSSH & WinRM
			- No agent to exploit or update
			- More efficient and secure

########################################################################################
	o	Ansible Introduction:
########################################################################################
		•	Describe the terminology and architecture of Ansible.
		
		https://docs.ansible.com/ansible/latest/dev_guide/overview_architecture.html
		
	
		
	Simple IT automation engine that automates 
		cloud provisioning, 
		configuration management, 
		application deployment, 
		intra-service orchestration
		etc..

	Designed for multi-tier deployments 
	Ansible models your IT infrastructure by describing 
		how systems inter-relate

	Has no agents 
	No additional custom security infrastructure
	It’s easy to deploy 
	Uses YAML as Ansible Playbooks
		describe automation jobs 
		close to plain English.




	How Ansible works?
	------------------
	Configuration file:
	/etc/ansible/ansible.cfg
	
		

	Modules
	-------
		Like commands - Ansible works by executing modules.
		Ansible works by 
			connecting to nodes 
			pushing out scripts called “Ansible modules” to them. 
		
		Most modules : describe the desired state of the system. 
		Executes these modules 
			(over SSH by default), 
		Removes them when finished. 
		Library of modules can reside on any machine
			No servers, daemons, or databases required.

		To write our own modules
		
			Check the following if there is already a module
			https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html
		Modules can be written in any language that can return JSON 
			(Ruby, Python, bash, etc).

	Module utilities
	----------------
		Multiple modules 
			can use the same code
			Ansible stores those functions as module utilities 
				minimize duplication 
				ease maintenance. 
			E.g., 
				code that parses URLs is lib/ansible/module_utils/url.py. 
				Module utilities 
					write in Python or in PowerShell.
					
		e.g. https://github.com/ansible/ansible-modules-core/blob/devel/system/user.py
		N.B: Ansible modules are NOT platform independent by default.
			But all (almost all) modules are written such that it will work on all modules.

	Plugins
	-------
		Execute on control node 
			within the /usr/bin/ansible process. 
			N.B: Modules execute on target system (usually remote) 
		Plugins offer extensions of features 
			e.g. 
			transforming data, 
			logging output, 
			connecting to inventory
		Plugins must be written in Python.
		Ansible ships default handy plugins
		We can add
		E.g., we can write inventory plugin to connect to any datasource that returns JSON. 	
		
	
	Inventory
	---------
	Default inventory file: /etc/ansible/hosts
	
	Machines Ansible manages in a file 
		(INI, YAML, etc.) 
	Categories of machines.
	Add/Update/Delete machines
		small entry in a file.
	Debugging hosts related issues became very easy.	
		
	Ansible can draw inventory, group, and variable information from sources like 
		EC2, 
		Rackspace, 
		OpenStack
		, and more.
	You can mention 
		IP
		Hostname
		FQDN
		in inventory file.

	Here’s what a plain text inventory file looks like:

-------------------------------------------------
---
[webservers]
www1.example.com
www2.example.com

[dbservers]
db0.example.com
db1.example.com
-------------------------------------------------

	Types of Inventory
		Static
			hardcoded ip, hostname or fqdn
		Dynamic
			Scripts for dynamic env.
				e.g. Shell, python etc.
			Cloud Public IP keeps changing on restart 
			Ansible implementation available for 
				AWS EC2
				Azure
				Openstack
				GCE
				Space Walk
				Jails 
				etc.
		Now the code can be found at
			https://raw.githubusercontent.com/ansible/ansible/stable-2.9/contrib/inventory/ec2.py


		Assigning variables to inventory
		--------------------------------
		Option1
		-------
		e.g. updating users for a group.
		1. Create one of the following directory
			/etc/ansible/group_vars/
			/etc/ansible/host_vars/
		2. Create a file matching the group name.
		
		Option2
		-------
		Update directly in the inventory file.

		Option3
		-------
		Use a dynamic inventory to pull your inventory from data sources like 
			EC2, 
			Rackspace, or 
			OpenStack.

	Playbooks
	---------
	Finely orchestrate infrastructure 
	Detailed control over machines in parallel. This is where Ansible starts to get most interesting.

	Ansible’s approach to orchestration is one of finely-tuned simplicity, as we believe your automation code should make perfect sense to you years down the road and there should be very little to remember about special syntax or features.

	Here’s what a simple playbook looks like:

	---
	- hosts: webservers
	serial: 5 # update 5 machines at a time
	roles:
	- common
	- webapp

	- hosts: content_servers
	roles:
	- common
	- content
	
	Ansible follows Push mechanism
	Ansible is agentless.
	
	The Ansible search path
	Following can live in multiple locations
		Modules
		module utilities
		plugins
		playbooks and 
		roles. 
	
	While extending Ansible’s core features
		we need multiple files with similar or the same names 
			in different locations
	This is managed using search path determines which of these files.
	
	Ansible’s search path grows incrementally
	Ansible finds 
		playbook and role included in a given run, 
			appends any directories related to that playbook or role to the search path. 
	Those directories remain in scope 
		for the duration of the run, 
		even after the playbook or role has finished executing. 
	
	
	Ansible loads modules, module utilities, and plugins in following order:
	Directories adjacent to a playbook specified on the command line. If you run Ansible with ansible-playbook /path/to/play.yml, Ansible appends these directories if they exist:

	/path/to/modules
	/path/to/module_utils
	/path/to/plugins
	Directories adjacent to a playbook that is statically imported by a playbook specified on the command line. If play.yml includes - import_playbook: /path/to/subdir/play1.yml, Ansible appends these directories if they exist:

	/path/to/subdir/modules
	/path/to/subdir/module_utils
	/path/to/subdir/plugins
	Subdirectories of a role directory referenced by a playbook. If play.yml runs myrole, Ansible appends these directories if they exist:

	/path/to/roles/myrole/modules
	/path/to/roles/myrole/module_utils
	/path/to/roles/myrole/plugins
	Directories specified as default paths in ansible.cfg or by the related environment variables, including the paths for the various plugin types. See Ansible Configuration Settings for more information. Sample ansible.cfg fields:

	DEFAULT_MODULE_PATH
	DEFAULT_MODULE_UTILS_PATH
	DEFAULT_CACHE_PLUGIN_PATH
	DEFAULT_FILTER_PLUGIN_PATH
	Sample environment variables:

	ANSIBLE_LIBRARY
	ANSIBLE_MODULE_UTILS
	ANSIBLE_CACHE_PLUGINS
	ANSIBLE_FILTER_PLUGINS
	The standard directories that ship as part of the Ansible distribution.

	Caution

	Modules, module utilities, and plugins in user-specified directories will override the standard versions. This includes some files with generic names. For example, if you have a file named basic.py in a user-specified directory, it will override the standard ansible.module_utils.basic.

	If you have more than one module, module utility, or plugin with the same name in different user-specified directories, the order of commands at the command line and the order of includes and roles in each play will affect which one is found and used on that particular play.
	
	

---------------------------------------------------------------------------
	How Ansible Works
---------------------------------------------------------------------------
https://www.ansible.com/overview/how-ansible-works

https://www.whizlabs.com/blog/how-ansible-works/

	1. Architecture

		modules: instructions to execute on the managed node 
		inventories: group of host/nodes
		plugins: improve the functionlity of Ansible. Execute on controller
		playbooks: Workflow of plays/instructions to execute on managed nodes.
		APIs: Helps in automation.
		
		When we execute a (ad-hoc) command
		Ansible Controller 
			Identifies the group of machines to work with
			Connects to the nodes 
			Pushes out the ansible modules. 
				Modules: 
					small programs that has to be executed on nodes.
					Perform tasks on the node
				Default: Modules get pushed to ~/.ansible/tmp directory
				A new directory would be created inside ~/.ansible/tmp
				The python code will get pushed into this.
				Python module would be executed.
			The above execution happens in parallel.
			Default: 5 threads
				Can be increased using -f in ad-hoc command.
				ansible.cfg also have forks. Modify this to increase the default.
		After the completion of the task, Ansible removes the modules.
			Set ANSIBLE_KEEP_REMOTE_FILES=1 to stop deleting pushed modules.
		Result is reported back to the control node.
		
		
-------------------------------------------------------
ANSIBLE_KEEP_REMOTE_FILES=1 ansible all -m shell -a "uptime"
Based on the user you used..
cd /root/.ansible/tmp
or
cd /home/vagrant/.ansible/tmp
-------------------------------------------------------

	2. SSH Keys
	Passwords are supported
		But they are hardly used.
		Nobody wants to hard code password in files.
	Authorized key
		Helps in Controller to connect and execute commands without password.


3. 
	Managing Inventory
	Ansible represents groups of machines in simple INI file. 
	
	Plugins
	Can easily plugin into 
		groups, 
		inventory, 
		other sources like 
			OpenStack, EC2, Rackspace, and more. 
	

4. Ansible Playbooks
	Orchestrating the infrastructure topology of the users. 
	The simplicity of automation with Ansible is a feature that makes it more demanding.

	
	########################################################################################	
		•	Understanding requirement.
	########################################################################################	
---------------------------------------------------------------------------
	o	Ansible installation:
---------------------------------------------------------------------------	
	Ansible 
		can run from any machine 
		Need
			Python 2 (version 2.7)  
			yum install python2
		or
			Python 3 (versions 3.5 and higher) 
			yum install python3
			
	OS can be		
		Red Hat, 
		Debian, 
		CentOS, 
		macOS, 
		BSDs (Berkeley Software Distribution)
			https://en.wikipedia.org/wiki/List_of_BSD_operating_systems
		
		N.B: Windows is not supported for the control node.

	Control node
		colated management system always works better. 
	
		Ansible in cloud
			Preferably control node also can be in cloud.
		Documentation: In most cases this will work better than on the open Internet.

		macOS 
			default configured for 15 nodes.
			to increase the limit 
				increase ulimit with 
					sudo launchctl limit maxfiles unlimited. 
				
	Managed node requirements
		Software required
			ssh
			Python 2/3
		Control node connects to managed nodes
			using SSH. 
				default : uses SFTP. 
				If required switch to SCP in ansible.cfg. 
			Also need 
				Python 2 (version 2.6 or later) or 
				Python 3 (version 3.5 or later).
				
---------------------------------------------------------------------------
	Select Ansible version
	----------------------
	Depends on what you need?

	Install latest release with OS package manager 
	For 
		RHEL (TM), 
		CentOS, 
		Fedora, 
		Debian, or 
		Ubuntu.

		Install with pip (the Python package manager).

N.B: Don't run Ansible from devel : This is a rapidly changing and unstable at any point.

	Ansible released 
		2/3 times a year. 
	minor bugs 
		fixed in next release 
	Major bugs 
		maintenance releases 
		infrequent.

	Installing Ansible on RHEL, CentOS, or Fedora
	
	On Fedora:
	$ sudo dnf install ansible
	
	On RHEL and CentOS:
	$ sudo yum install epel-release
	$ sudo yum install python-argcomplete
	$ sudo yum install ansible			
			
			sudo activate-global-python-argcomplete
			
			
	Configuring Ansible Host
	------------------------
	
	All hosts/nodes are maintained in  
		/etc/ansible/hosts
		“hosts” file. 
	Set it up as follows
	
	sudo vi /etc/ansible/hosts
-------------------------------------------------	
[myansible]
master ansible_ssh_host=192.168.176.142

[servers]
host2 ansible_ssh_host=192.168.176.143
host3 ansible_ssh_host=192.168.176.144
-------------------------------------------------

server_group_name 
	e.g. myansible and servers
	organizational tag 
	refer to any servers listed under it with one word. 
	
alias
	e.g. master, host2, host3
	name to refer to that server.


ansible -m ping all
	It fails
	We need to setup two more things
	
	1. Setup user
		e.g. root@
	2. Setup password less access 
		using ssh keys.
		
Setup user
----------
	sudo mkdir /etc/ansible/group_vars
	sudo vi /etc/ansible/group_vars/servers

-----------------------------------
ansible_ssh_user: vagrant	
-----------------------------------
#--- is critical don't ignore that.

Now for the group servers, ansible will try to connect using vagrant

Setup password less access
--------------------------
----------------------------------------------------
On the controller generate keys
	ssh-keygen -t rsa -C vagrant@192.168.176.142
	
	cd /home/vagrant/.ssh
	ls -a
	
	ssh-copy-id vagrant@192.168.176.143
	ssh-copy-id vagrant@192.168.176.144

	ssh-copy-id root@192.168.176.143
	ssh-copy-id root@192.168.176.144

----------------------------------------------------

ansible -m ping all
ansible -m ping servers
ansible -m ping servers,myansible # group name. No space supported.


	
Using Notepad++ for YAML editing
http://xbmcnut.blogspot.com/2016/10/my-notepad-tricks-when-editing-yaml.html
https://community.home-assistant.io/t/tips-for-editing-yaml-with-notepad/4295	
	
########################################################################################
		•	Install Ansible, Select platform, Check option, Understand differences.
		
		https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
		On the left side there are options for each OS.
		
########################################################################################		

----------------------------------------------------------------------------------------------
Ansible language basics
----------------------------------------------------------------------------------------------
	Playbooks
		Plain text YAML files that describe the desired state of something.
		YAML: Human and machine readble
		Can be used to build entire application environments
		- Contain plays
			- Plays contain tasks
				- Tasks calls modules
				- Tasks run sequentially
				- Trigger Handlers and are run once at the end of plays.
	Variables
		like variables in language
			
	Inventories
		- Used to define servers/machines.
		- Static lines of servers
		- Ranges
		- Other custom things
		- Dynamic list of servers: AWS, Azure, GCP etc
	

----------------------------------------------------------------------------------------------


		•	Run ad hoc commands
		
		https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html
		
		Managed through "command-line" ansible 
			/usr/bin/ansible  
		Used to automate a single task 
			on 1/more managed nodes. 
		Quick and easy
		Not reusable. 
		e.g. 
			Power off all the machines 
			reboot servers, 
			copy files, 
			manage packages and users
			etc

		Standard ansible ad hoc command pattern
			$ ansible [pattern] -m [module] -a "[module options]"
			
			e.g 
				ansible all -m ping 
				ansible frontend -m ping
		
		Rebooting servers
		-----------------
	
	Default module for ansible CLI 
		command module. 
	
	e.g. Reboot all web servers in Servers together. 
	Steps
	1. All servers in "Servers" listed in a a group called [servers] in your inventory
	2. SSH access from controller to each machine in that group. 	
---------------------------------------------------------------	
		$ ansible servers -a "/sbin/reboot"
		
		host2 | UNREACHABLE! => {
    "changed": false,
	message like above is natural as the node is restarting while ansible is at work.
---------------------------------------------------------------	
		
	Modifying default 
	-----------------
	Use adhoc commands for this.
		
	a.	Ansible uses 5 simultaneous processes (nodes). 
	
		Reboot the [servers] servers with 10 parallel forks:
		$ ansible servers -a "/sbin/reboot" -f 10

	b. /usr/bin/ansible will default to running from your user account. 
	
		Connect as a different user:
		$ ansible servers -a "/sbin/reboot" -f 10 -u username

	c. Rebooting probably requires privilege escalation. 
		Connect as root and run command 
		$ ansible servers -a "/sbin/reboot" -f 10 -u username --become [--ask-become-pass]
		
		--ask-become-pass or -K, 
			Prompted for a password 
			Use privilege escalation (sudo/su/pfexec/doas/etc).

	d. Command module 
		Doesn't support extended shell syntax 
			like piping and 
			redirects. 
		To execute shell-specific syntax, 
			use the shell module. 
			
		To use different module, 
			pass -m
			$ ansible raleigh -m shell -a 'echo $TERM'

				Understand shell quoting rules
				------------------------------
				When running any command with Ansible 
					ad hoc CLI (as opposed to Playbooks), 
					pay particular attention to shell quoting rules
					For e.g., 
					'echo $TERM'
						$TERM will be evaluated on the client node
					"echo $TERM"
						$TERM will be evaluated on the node where it is executed (master)
			

	e. Managing files
		Through Ansible and SCP 
		
		e1. transfer many files to multiple machines in parallel. 
		
			To transfer a file directly to all servers in the [servers] group:
				$ ansible servers -m copy -a "src=/etc/hosts dest=/tmp/hosts"

			To repeat the above: use template in playbook.

		
		e2. change ownership and permissions on files. 
			$ ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"
			$ ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=vilas group=dev"

		
		e3. create directories, similar to mkdir -p:
			Use state=directory
			$ ansible webservers -m file -a "dest=/path/to/c mode=755 owner=vilas group=dev state=directory"
		
		e4. Delete directories (recursively) and delete files:
			Use state=absent
			$ ansible webservers -m file -a "dest=/path/to/c state=absent"

	
	f. Managing packages
		install, update, or remove packages on managed nodes 
			like using yum. 
		
		f1. Ensure package is installed without updating it:
			$ ansible webservers -m yum -a "name=acme state=present"
			

		f2. Install specific version:
			$ ansible webservers -m yum -a "name=acme-1.5 state=present"

		f3. Ensure latest version for a package:
			$ ansible webservers -m yum -a "name=acme state=latest"

		f4. Ensure a package is not installed:
			$ ansible webservers -m yum -a "name=acme state=absent"

Ansible has modules for managing packages under many platforms. If there is no module for your package manager, you can install packages using the command module or create a module for your package manager.

Managing users and groups
You can create, manage, and remove user accounts on your managed nodes with ad-hoc tasks:

$ ansible all -m user -a "name=foo password=<crypted password here>"

$ ansible all -m user -a "name=foo state=absent"
See the user module documentation for details on all of the available options, including how to manipulate groups and group membership.

Managing services
Ensure a service is started on all webservers:

$ ansible webservers -m service -a "name=httpd state=started"
Alternatively, restart a service on all webservers:

$ ansible webservers -m service -a "name=httpd state=restarted"
Ensure a service is stopped:

$ ansible webservers -m service -a "name=httpd state=stopped"
Gathering facts
Facts represent discovered variables about a system. You can use facts to implement conditional execution of tasks but also just to get ad-hoc information about your systems. To see all facts:

$ ansible all -m setup
You can also filter this output to display only certain facts, see the setup module documentation for details.

Now that you understand the basic elements of Ansible execution, you are ready to learn to automate repetitive tasks using Ansible Playbooks.

Ad-hoc tasks, like playbooks, use a declarative model, calculating and executing the actions required to reach a specified final state. They achieve a form of idempotence by checking the current state before they begin and doing nothing unless the current state is different from the specified final state.

More reference on adhoc commands 
https://ansible-tips-and-tricks.readthedocs.io/en/latest/ansible/commands/
########################################################################################

	o	What are playbooks?
	
	Ansible’s configuration, deployment, and orchestration language. 
	Can describe a policy 
		enforce it on remote systems
	Designed to be human-readable 
		Developed in simple language. 


	
	Policy
		simple: manage configurations and deployments of remote machines. 
		advanced: sequence multi-tier rollouts with rolling updates
					can delegate actions to other hosts
					interacting with monitoring servers and load balancers.


----------------------------------------------------------------
Take a look at 
	simple ansible playbook
	complex ansible playbook
----------------------------------------------------------------

	Playbooks
	---------
	basis for 
		simple configuration management 
		multi-machine deployment system

	can declare configurations
	can orchestrate manual ordered process
		with steps move between sets of machines in particular orders
	can launch tasks synchronously or asynchronously.
-------------------------------------------------------------
	Recollect: ad-hoc tasks: run /usr/bin/ansible program 
-------------------------------------------------------------
	
	Mostly maintained in 
		files
			yaml
		source controlled
		push out your configuration 
		assure configurations of your remote systems are in spec.

official ansible git repo's
	https://github.com/ansible/
	https://github.com/ansible/ansible-examples
	https://github.com/ansible/ansible-blog-examples


	Playbook Language 
	-----------------
	Expressed in YAML format (see YAML Syntax) 
	Have minimum of syntax
		designed to make it simple
			configuration 
			process.
		not 
			programming language 
			script

	
	Each playbook 
		one or more ‘plays’ in a list.

	Each Play
		Do tasks
			Maps group of hosts to roles
			Call to ansible module.

	Composing playbook 
		of multiple ‘plays’, 
		orchestrate multi-machine deployments
		running 
			certain steps on group A
			certain steps on group B
			return back to execute few more commands on group A

	What is YAML?
########################################################################################
		•	Implement playbooks.
		
Playbooks 
	Basis for 
		Simple configuration management and 
		Multi-machine deployment system
			Very well suited to deploying complex applications.

	Declare configurations
	Orchestrate steps of any manual ordered process
	Do complex operations like
		different steps must bounce back and forth between sets of machines in particular orders. 
	Launch tasks synchronously or asynchronously.

N.B: ad-hoc tasks: run /usr/bin/ansible program
Playbooks 
	Generally kept in source control 
	Used to push out your configuration 
	Assure the configurations of your remote systems are in spec.

	Expressed in YAML format 
	Not a programming language or script, 
	Just a configuration or a process.
	
	
	
	Each playbook is composed of 
		one or more ‘plays’ in a list.

	Goal of play is to 
		map a group of hosts to some well defined roles, 
			represented by tasks. 
	
	At basic level Task is 
		just a call to ansible module.

	By composing a playbook of multiple ‘plays’, it is possible to orchestrate multi-machine deployments, running certain steps on all machines in the webservers group, then certain steps on the database server group, then more commands back on the webservers group, etc.

“plays” are more or less a sports analogy. You can have quite a lot of plays that affect your systems to do different things. It’s not as if you were just defining one particular state or model, and you can run different plays at different times.

For starters, here’s a playbook, verify-apache.yml that contains just one play:
	
	
	
		Fill up more details from Ansible documentation
########################################################################################		
		•	Write Ansible plays and execute a playbook.
		
Each playbook is composed of 
	1(+) ‘plays’ in a list.

Play 
	Map group of hosts to well defined roles called tasks
	
Task	
	Simplest task: call to an ansible module.
e.g. 
Compose a playbook of multiple ‘plays’
	We can orchestrate multi-machine deployments
	Running certain steps on all machines in group1, 
	then certain steps on group2, 
	then more commands back on group1, etc.



Playbooks 
	Orchestrate steps on multiple nodes 
		get them all to a certain desired state.
	Define variables, configurations, deployment steps, 
		assign roles, perform multiple tasks. 
	
	e.g. COPY / DELETE Files and Folders, install packages, start services.
		Can do lot more complicated operations

	Written in YAML format. 
	Human-readable data serialization language. 
	
	For Ansible
		Every YAML file starts with a list. 
		Explain YAML if not already covered.
		
	Every playbook starts with 3 sections ('—'s)
	Host section – 
		Target machines on which the playbook should run. 
		Based on the Ansible inventory file.
		list one or more groups or host patterns
			separated by colons.
		May include Remote user 
				name of the user account
	Variable section
		Optional 
		can declare all the variables needed in the playbook. 
	Tasks section 
		Lists out sequence of tasks to be executed on the target machine. 
		Uses Modules. 
		Every task has 
			name: small description of the task 
			will be listed while the playbook is run.
		May include Handlers: 
			 Special Task with notify directive 
			 Indicates that it changed something. 
			 For e.g., if a config file is changed, 
				then task referencing the config file may 
				notify a service restart handler.

		


########################################################################################		
		•	Manage variables and inclusions
########################################################################################
		•	Describe variable scope and precedence; manage variables and facts in a play.
		
		Used to store values in programs 
		Values can be changed throughout the program. 
		Can be used in deciding code flow. 
		
		Ansible variables help to determine 
			how the tasks execute on different systems 
			based on the values assigned to these variables.




		
			Variables
		Variables can be used from different sources
			- Playbooks
			- Files
			- Inventories
			- Command line
			- Discovered variables
			- Ansible Tower

		Variables:
		----------
		Makes playbooks and roles more flexible. 
		Can be used to loop through a set of given values
		Access various information like the 
			host name 
			replace certain strings in templates with specific values.

		Ansible defines a rich set of variables
		All facts about system are gathered and set as variables.

		Rule for naming variables.
		Variables should always start with a letter.	
		Followed 
			letters, 
			numbers, and 
			underscores. E.g. wamp_21, port5 is valid variable names, whereas 01_port, _server are invalid.

			Variables
		Variables can be used from different sources
			- Playbooks
			- Files
			- Inventories
			- Command line
			- Discovered variables
			- Ansible Tower


########################################################################################		
		•	Implement parallelism
		https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html
		
		Working with multiple nodes 
		Can imporve the efficiency by 
			applying configurations parllaly.
		
		Default, 
			Ansible runs each task on hosts parallely 
				uses 5 forks
				starts next task on next host
		To change default behavior
			a. using another strategy plugin
				Not for managing parallel execution.
			b. change the number of forks
				forks/-f 
			c. apply a play-level keywords like serial.
				serial
			
			
		Ansible Strategy Plugins
		------------------------
		1. Linear
			Default
			Execute 5 forks (threads) 
			Executes each tasks on 5 nodes.
			Wait until all 5 are over
			Once all 5 are over, proceed with the next host
		2. Free
			Start with 5 forks on 5 nodes.
			If one thread completes the job,
				don't wait 
				proceed with the next set of execution.
			Fastest execution strategy
		3. Debug
		https://docs.ansible.com/ansible/2.4/playbooks_debugger.html
			Enables you to invoke a debugger when a task has failed. 
			You get access to entire debugger features when a task fails. 
			Then, 
				a. check or set the value of variables
				b. update module arguments, and 
				c. re-run the failed task with the new variables and arguments 
					
					
		Change the number of forks
		--------------------------
		1. Update forks in ansible.cfg
			Default: /etc/ansible/ansible.cfg
			Can be overriden by: ~/ansible.cfg
		2. Pass along with command line argument
			ansible-playbook -f 10 playbook.yml
			ansible -f 10 -m ping all
		
		play-level keywords 
		-------------------
		Several play-level keyword also affect play execution. 
		serial sets 
			a number, 
			a percentage, or 
			a list of numbers of hosts 
		you want to manage at a time. 
		Setting serial with any strategy 
			directs Ansible to ‘batch’ the hosts, 
			completing the play on the specified 
				number or 
				percentage of hosts 
			before starting the next ‘batch’. 
		Especially useful for rolling updates.


		Other most used play-level commands 
			Generally these are not related to parallel execution.
			
			throttle
				Used for rate limiting - mostly for CPU intensive commands.
			ignore_errors
			ignore_unreachable
			any_errors_fatal

	Make a playbook execute sequentially 
		Set serial to 1
		ansible-playbook serial.yaml

########################################################################################
	o	Implement task control
########################################################################################
		•	Manage task control, handlers, and tags in Ansible playbooks.
########################################################################################		
			
			
			
		•	Use of decision statements, ‘when’, etc.
########################################################################################		
		•	Some common and important modules
########################################################################################
	o	Ansible for Windows infra management
########################################################################################
		•  Introduction to win modules.
########################################################################################
	o	Using Jinja2 templates o What are Ansible Roles?
########################################################################################
		•	Create and manage roles.
########################################################################################		
		•	Code portability across multiple control servers.
########################################################################################
	o	What is Ansible vault?
########################################################################################
		•	Implement Ansible Vault
########################################################################################		
		•	Manage encryption with Ansible Vault.
########################################################################################
	o	LAB: Implement Ansible in a DevOps environment.
########################################################################################
		•	Integrate with CI / CD tools etc.
########################################################################################		
		•	Cloud Infrastructure provisioning.
########################################################################################		
		•	Patch Management.
########################################################################################


################################################################################
		Additional concepts (Not part of TOC)
################################################################################

	Additional concepts: How to build your inventory?
	--------------------------------------------------
	Default ansible configuration file : /etc/ansible/ansible.cfg
	e.g. inventory location can be modified from /etc/ansible/ansible.cfg
	
	Also we can have a user defined configuration defined in 
	~/.ansible.cfg
		If defined above file will override /etc/ansible/ansible.cfg


	Define the group of machines (remote node/hosts) which you want to manage together in a group
	Machines can belong to multiple groups.

	Default inventory location : /etc/ansible/hosts. 
	Different inventory file can be selected 
		-i <path> in CLI while executing command. 
	Supports multiple inventory files at the same time
		pull inventory from dynamic or cloud sources 
	Different formats 
		(YAML, ini, etc), 
	Ansible has Inventory Plugins to make this flexible and customizable.
		Introduced in version 2.4




Depending on the inventory plugins we have
	Inventory file can be in any one format.
	
Most common formats 
	INI and YAML. 
	
Basic INI etc/ansible/hosts 

-----------------------------------------------------
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
-----------------------------------------------------

headings in brackets 
	group names, 
	Used in classifying hosts and deciding what hosts you are controlling at what times and for what purpose.

Here’s that same basic inventory file in YAML format:

-----------------------------------------------------
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

-----------------------------------------------------

Default/Implicit groups
Two default groups: 
	all 
		every host		
	ungrouped. 
		all hosts that don’t have another group aside from all. 
	
	Every host will always belong to at least 2 groups 
	
Hosts in multiple groups
You can (and probably will) put each host in more than one group. 

Best practices create groups that track:
	What - 
		An application, stack or microservice. 
		e.g database servers, web servers, etc.
	Where - 
		Datacenter or region, to talk to local DNS, storage, etc. (For example, east, west).
	When - 
		Development, stage, prod, test.
		
e.g.
------------------------------------------------------------------------	
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
------------------------------------------------------------------------
one.example.com exists in 
	dbservers
	east and 
	prod groups.

	nested groups can simplify prod and test above:
-------------------------------------------------------------
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
      children:
        east:
    test:
      children:
        west:
-------------------------------------------------------------


Adding ranges of hosts
----------------------
Lot of hosts with a similar pattern
	add them as a range rather 

In INI:
----------------------------------------
[webservers]
www[01:50].example.com
In YAML:
----------------------------------------
...
  webservers:
    hosts:
      www[01:50].example.com:
----------------------------------------	  
For numeric patterns
	leading zeros can be included or removed, as desired. 
	Ranges are inclusive. 

alphabetic ranges:
------------------------------------------
[databases]
db-[a:f].example.com
------------------------------------------

Variables to inventory
----------------------
	Variables can be 
		1. defined and used in a inventory file
		2. Can be defined in a seperate file and used in inventory
		
Can store variable values that relate to a 
	specific host or group in inventory. 
Adding variables directly to host/group.
	in inventory file



Assigning a variable to one machine: host variables

assign a variable to a single host
	then use it later in playbooks. 
	
In INI:
------------------------------------------------
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=909
------------------------------------------------
or 

Same in YAML:
------------------------------------------------
atlanta:
  host1:
    http_port: 80
    maxRequestsPerChild: 808
  host2:
    http_port: 303
    maxRequestsPerChild: 909
------------------------------------------------	

Ports as hot variables. 

	badwolf.example.com:5309

Connection variables / Username 
[targets]
----------------------------------------------------------------------
localhost              ansible_connection=local
other1.example.com     ansible_connection=ssh        ansible_user=myuser
other2.example.com     ansible_connection=ssh        ansible_user=myotheruser
----------------------------------------------------------------------

Note

N.B: 
	non-standard SSH ports in your SSH config file
		openssh connection will find and use them
		but the paramiko connection will not.


Inventory aliases
Define aliases in your inventory:

In INI:
----------------------------------------------------
jumper ansible_port=5555 ansible_host=192.0.2.50
----------------------------------------------------
or
In YAML:
----------------------------------------------------
...
  hosts:
    jumper:
      ansible_port: 5555
      ansible_host: 192.0.2.50
----------------------------------------------------
	  
Above running Ansible 
	against host 
	alias “jumper” 
	connect to IP 192.0.2.50 
		on port 5555. 
		
		
Note

In INI format using the 
	key=value syntax 
	are interpreted differently 
	depending on where they are declared:

When declared inline with the host
	INI values are interpreted as Python literal structures 
		(strings, numbers, tuples, lists, dicts, booleans, None). 
	Host lines accept multiple 
		key=value parameters per line. 
	So space can be 
		separator
		part of value
When declared in a :vars section, INI values are interpreted as strings. For example var=FALSE would create a string equal to ‘FALSE’. Unlike host lines, :vars sections accept only a single entry per line, so everything after the = must be the value for the entry.
If a variable value set in an INI inventory must be a certain type (for example, a string or a boolean value), always specify the type with a filter in your task. Do not rely on types set in INI inventories when consuming variables.
Consider using YAML format for inventory sources to avoid confusion on the actual type of a variable. The YAML inventory plugin processes variable values consistently and correctly.
Generally speaking, this is not the best way to define variables that describe your system policy. Setting variables in the main inventory file is only a shorthand. See Organizing host and group variables for guidelines on storing variable values in individual files in the ‘host_vars’ directory.

Assigning a variable to many machines: group variables

In INI:
-------------------------------------------
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com

-------------------------------------------

In YAML:
-------------------------------------------
atlanta:
  hosts:
    host1:
    host2:
  vars:
    ntp_server: ntp.atlanta.example.com
    proxy: proxy.atlanta.example.com
-------------------------------------------
	
Group variables 
	Convenient way to apply variables to multiple hosts at once. 
	Before executing
		Ansible always flattens variables, 
			including inventory variables, 
			to the host level. 
	If a host is a member of multiple groups, 
		Ansible reads variable values from all of those groups. 
		If you assign different values to the same variable in different groups, 
		Ansible chooses which value to use based on internal rules for merging.

Inheriting variable values: 
	group variables for groups of groups
	You can make groups of groups using the 
		:children suffix in INI or the 
		children: entry in YAML. 
	Use	:vars or vars:: to apply variables to groups of groups 

In INI:
------------------------------------------------------------
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
------------------------------------------------------------

In YAML:
------------------------------------------------------------
all:
  children:
    usa:
      children:
        southeast:
          children:
            atlanta:
              hosts:
                host1:
                host2:
            raleigh:
              hosts:
                host2:
                host3:
          vars:
            some_server: foo.southeast.example.com
            halon_system_timeout: 30
            self_destruct_countdown: 60
            escape_pods: 2
        northeast:
        northwest:
        southwest:
------------------------------------------------------------		
If you need to store lists or hash data, or prefer to keep host and group specific variables separate from the inventory file, see Organizing host and group variables.

Child groups have a couple of properties to note:

Any host that is member of a child group is automatically a member of the parent group.
A child group’s variables will have higher precedence (override) a parent group’s variables.
Groups can have multiple parents and children, but not circular relationships.
Hosts can also be in multiple groups, but there will only be one instance of a host, merging the data from the multiple groups.
Organizing host and group variables
Although you can store variables in the main inventory file, storing separate host and group variables files may help you organize your variable values more easily. Host and group variable files must use YAML syntax. Valid file extensions include ‘.yml’, ‘.yaml’, ‘.json’, or no file extension. See YAML Syntax if you are new to YAML.

Ansible loads host and group variable files by searching paths relative to the inventory file or the playbook file. If your inventory file at /etc/ansible/hosts contains a host named ‘foosball’ that belongs to two groups, ‘raleigh’ and ‘webservers’, that host will use variables in YAML files at the following locations:

/etc/ansible/group_vars/raleigh # can optionally end in '.yml', '.yaml', or '.json'
/etc/ansible/group_vars/webservers
/etc/ansible/host_vars/foosball
For example, if you group hosts in your inventory by datacenter, and each datacenter uses its own NTP server and database server, you can create a file called /etc/ansible/group_vars/raleigh to store the variables for the raleigh group:

---
ntp_server: acme.example.org
database_server: storage.example.org
You can also create directories named after your groups or hosts. Ansible will read all the files in these directories in lexicographical order. An example with the ‘raleigh’ group:

/etc/ansible/group_vars/raleigh/db_settings
/etc/ansible/group_vars/raleigh/cluster_settings
All hosts in the ‘raleigh’ group will have the variables defined in these files available to them. This can be very useful to keep your variables organized when a single file gets too big, or when you want to use Ansible Vault on some group variables.

You can also add group_vars/ and host_vars/ directories to your playbook directory. The ansible-playbook command looks for these directories in the current working directory by default. Other Ansible commands (for example, ansible, ansible-console, etc.) will only look for group_vars/ and host_vars/ in the inventory directory. If you want other commands to load group and host variables from a playbook directory, you must provide the --playbook-dir option on the command line. If you load inventory files from both the playbook directory and the inventory directory, variables in the playbook directory will override variables set in the inventory directory.

Keeping your inventory file and variables in a git repo (or other version control) is an excellent way to track changes to your inventory and host variables.

How variables are merged
By default variables are merged/flattened to the specific host before a play is run. This keeps Ansible focused on the Host and Task, so groups don’t really survive outside of inventory and host matching. By default, Ansible overwrites variables including the ones defined for a group and/or host (see DEFAULT_HASH_BEHAVIOUR). The order/precedence is (from lowest to highest):

all group (because it is the ‘parent’ of all other groups)
parent group
child group
host
By default Ansible merges groups at the same parent/child level alphabetically, and the last group loaded overwrites the previous groups. For example, an a_group will be merged with b_group and b_group vars that match will overwrite the ones in a_group.

You can change this behavior by setting the group variable ansible_group_priority to change the merge order for groups of the same level (after the parent/child order is resolved). The larger the number, the later it will be merged, giving it higher priority. This variable defaults to 1 if not set. For example:

a_group:
    testvar: a
    ansible_group_priority: 10
b_group:
    testvar: b
In this example, if both groups have the same priority, the result would normally have been testvar == b, but since we are giving the a_group a higher priority the result will be testvar == a.

Note

ansible_group_priority can only be set in the inventory source and not in group_vars/, as the variable is used in the loading of group_vars.

Using multiple inventory sources
You can target multiple inventory sources (directories, dynamic inventory scripts or files supported by inventory plugins) at the same time by giving multiple inventory parameters from the command line or by configuring ANSIBLE_INVENTORY. This can be useful when you want to target normally separate environments, like staging and production, at the same time for a specific action.

Target two sources from the command line like this:

ansible-playbook get_logs.yml -i staging -i production
Keep in mind that if there are variable conflicts in the inventories, they are resolved according to the rules described in How variables are merged and Variable precedence: Where should I put a variable?. The merging order is controlled by the order of the inventory source parameters. If [all:vars] in staging inventory defines myvar = 1, but production inventory defines myvar = 2, the playbook will be run with myvar = 2. The result would be reversed if the playbook was run with -i production -i staging.

Aggregating inventory sources with a directory

You can also create an inventory by combining multiple inventory sources and source types under a directory. This can be useful for combining static and dynamic hosts and managing them as one inventory. The following inventory combines an inventory plugin source, a dynamic inventory script, and a file with static hosts:

inventory/
  openstack.yml          # configure inventory plugin to get hosts from Openstack cloud
  dynamic-inventory.py   # add additional hosts with dynamic inventory script
  static-inventory       # add static hosts and groups
  group_vars/
    all.yml              # assign variables to all hosts
You can target this inventory directory simply like this:

ansible-playbook example.yml -i inventory
It can be useful to control the merging order of the inventory sources if there’s variable conflicts or group of groups dependencies to the other inventory sources. The inventories are merged in alphabetical order according to the filenames so the result can be controlled by adding prefixes to the files:

inventory/
  01-openstack.yml          # configure inventory plugin to get hosts from Openstack cloud
  02-dynamic-inventory.py   # add additional hosts with dynamic inventory script
  03-static-inventory       # add static hosts
  group_vars/
    all.yml                 # assign variables to all hosts
If 01-openstack.yml defines myvar = 1 for the group all, 02-dynamic-inventory.py defines myvar = 2, and 03-static-inventory defines myvar = 3, the playbook will be run with myvar = 3.

For more details on inventory plugins and dynamic inventory scripts see Inventory Plugins and Working with dynamic inventory.

Connecting to hosts: behavioral inventory parameters
As described above, setting the following variables control how Ansible interacts with remote hosts.

Host connection:

Note

Ansible does not expose a channel to allow communication between the user and the ssh process to accept a password manually to decrypt an ssh key when using the ssh connection plugin (which is the default). The use of ssh-agent is highly recommended.

ansible_connection
Connection type to the host. This can be the name of any of ansible’s connection plugins. SSH protocol types are smart, ssh or paramiko. The default is smart. Non-SSH based types are described in the next section.
General for all connections:

ansible_host
	The name of the host to connect to, if different from the alias you wish to give to it.
ansible_port
	The connection port number, if not the default (22 for ssh)
ansible_user
	The user name to use when connecting to the host
ansible_password
	The password to use to authenticate to the host (never store this variable in plain text; always use a vault. See Variables and Vaults)
Specific to the SSH connection:

ansible_ssh_private_key_file
	Private key file used by ssh. Useful if using multiple keys and you don’t want to use SSH agent.
ansible_ssh_common_args
	This setting is always appended to the default command line for sftp, scp, and ssh. Useful to configure a ProxyCommand for a certain host (or group).
ansible_sftp_extra_args
	This setting is always appended to the default sftp command line.
ansible_scp_extra_args
	This setting is always appended to the default scp command line.
ansible_ssh_extra_args
	This setting is always appended to the default ssh command line.
ansible_ssh_pipelining
	Determines whether or not to use SSH pipelining. This can override the pipelining setting in ansible.cfg.
ansible_ssh_executable (added in version 2.2)
	This setting overrides the default behavior to use the system ssh. This can override the ssh_executable setting in ansible.cfg.
Privilege escalation (see Ansible Privilege Escalation for further details):

ansible_become
Equivalent to ansible_sudo or ansible_su, allows to force privilege escalation
ansible_become_method
Allows to set privilege escalation method
ansible_become_user
Equivalent to ansible_sudo_user or ansible_su_user, allows to set the user you become through privilege escalation
ansible_become_password
Equivalent to ansible_sudo_password or ansible_su_password, allows you to set the privilege escalation password (never store this variable in plain text; always use a vault. See Variables and Vaults)
ansible_become_exe
Equivalent to ansible_sudo_exe or ansible_su_exe, allows you to set the executable for the escalation method selected
ansible_become_flags
Equivalent to ansible_sudo_flags or ansible_su_flags, allows you to set the flags passed to the selected escalation method. This can be also set globally in ansible.cfg in the sudo_flags option
Remote host environment parameters:

ansible_shell_type
The shell type of the target system. You should not use this setting unless you have set the ansible_shell_executable to a non-Bourne (sh) compatible shell. By default commands are formatted using sh-style syntax. Setting this to csh or fish will cause commands executed on target systems to follow those shell’s syntax instead.
ansible_python_interpreter
The target host python path. This is useful for systems with more than one Python or not located at /usr/bin/python such as *BSD, or where /usr/bin/python is not a 2.X series Python. We do not use the /usr/bin/env mechanism as that requires the remote user’s path to be set right and also assumes the python executable is named python, where the executable might be named something like python2.6.
ansible_*_interpreter
Works for anything such as ruby or perl and works just like ansible_python_interpreter. This replaces shebang of modules which will run on that host.
New in version 2.1.

ansible_shell_executable
This sets the shell the ansible controller will use on the target machine, overrides executable in ansible.cfg which defaults to /bin/sh. You should really only change it if is not possible to use /bin/sh (i.e. /bin/sh is not installed on the target machine or cannot be run from sudo.).
Examples from an Ansible-INI host file:

some_host         ansible_port=2222     ansible_user=manager
aws_host          ansible_ssh_private_key_file=/home/example/.ssh/aws.pem
freebsd_host      ansible_python_interpreter=/usr/local/bin/python
ruby_module_host  ansible_ruby_interpreter=/usr/bin/ruby.1.9.3
Non-SSH connection types
As stated in the previous section, Ansible executes playbooks over SSH but it is not limited to this connection type. With the host specific parameter ansible_connection=<connector>, the connection type can be changed. The following non-SSH based connectors are available:

local

This connector can be used to deploy the playbook to the control machine itself.

docker

This connector deploys the playbook directly into Docker containers using the local Docker client. The following parameters are processed by this connector:

ansible_host
The name of the Docker container to connect to.
ansible_user
The user name to operate within the container. The user must exist inside the container.
ansible_become
If set to true the become_user will be used to operate within the container.
ansible_docker_extra_args
Could be a string with any additional arguments understood by Docker, which are not command specific. This parameter is mainly used to configure a remote Docker daemon to use.
Here is an example of how to instantly deploy to created containers:

- name: create jenkins container
  docker_container:
    docker_host: myserver.net:4243
    name: my_jenkins
    image: jenkins

- name: add container to inventory
  add_host:
    name: my_jenkins
    ansible_connection: docker
    ansible_docker_extra_args: "--tlsverify --tlscacert=/path/to/ca.pem --tlscert=/path/to/client-cert.pem --tlskey=/path/to/client-key.pem -H=tcp://myserver.net:4243"
    ansible_user: jenkins
  changed_when: false

- name: create directory for ssh keys
  delegate_to: my_jenkins
  file:
    path: "/var/jenkins_home/.ssh/jupiter"
    state: directory
For a full list with available plugins and examples, see Plugin List.

Note

If you’re reading the docs from the beginning, this may be the first example you’ve seen of an Ansible playbook. This is not an inventory file. Playbooks will be covered in great detail later in the docs.

Inventory setup examples
Example: One inventory per environment
If you need to manage multiple environments it’s sometimes prudent to have only hosts of a single environment defined per inventory. This way, it is harder to, for instance, accidentally change the state of nodes inside the “test” environment when you actually wanted to update some “staging” servers.

For the example mentioned above you could have an inventory_test file:

[dbservers]
db01.test.example.com
db02.test.example.com

[appservers]
app01.test.example.com
app02.test.example.com
app03.test.example.com
That file only includes hosts that are part of the “test” environment. Define the “staging” machines in another file called inventory_staging:

[dbservers]
db01.staging.example.com
db02.staging.example.com

[appservers]
app01.staging.example.com
app02.staging.example.com
app03.staging.example.com
To apply a playbook called site.yml to all the app servers in the test environment, use the following command:

ansible-playbook -i inventory_test site.yml -l appservers
Example: Group by function
In the previous section you already saw an example for using groups in order to cluster hosts that have the same function. This allows you, for instance, to define firewall rules inside a playbook or role without affecting database servers:

- hosts: dbservers
  tasks:
  - name: allow access from 10.0.0.1
    iptables:
      chain: INPUT
      jump: ACCEPT
      source: 10.0.0.1
Example: Group by location
Other tasks might be focused on where a certain host is located. Let’s say that db01.test.example.com and app01.test.example.com are located in DC1 while db02.test.example.com is in DC2:

[dc1]
db01.test.example.com
app01.test.example.com

[dc2]
db02.test.example.com
In practice, you might even end up mixing all these setups as you might need to, on one day, update all nodes in a specific data center while, on another day, update all the application servers no matter their location.

	
	
	Additional concepts: Ansible Facts
	-----------------------------------
Information about Managed Nodes
	OS
	Distribution
	Release
	Processor
	python home 
	domain name
	ect..
	
	Task of collecting these called Gathering Facts
	
	adhoc command for gather facts
		ansible group -m setup 
			returns both default and custom facts.
		
	Types of Ansible Facts
		Default
		Custom
			User defined facts
			Can simplify playbooks and other operations.
			
	