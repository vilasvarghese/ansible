Ansible Templates
	manage configurations of multiple servers and environments. 
	Configuration files can vary for each 
		cluster or 
		remote server. 
	But apart from a few parameters, all other settings will be the same.
	
	Allow us to 
		create a template
		copy them to remote servers
		modify managed server (host) specific variables 
		execute the template there
	

Creating static files (templates) for each of these configurations will have lot of duplicate entries. 
Templates provides a way to manage dynamic values

A template 
	file that contains all your configuration parameters, 
	but the dynamic values are given as variables in the Ansible. 
	During the playbook execution, 
		it depends on the conditions such as which cluster you are using, 
		and the variables will be replaced with the relevant values.

Jinja2 templating engine supports
	replace the variables  
	loops, 
	conditional statements, 
	macros
	filters for transforming the data
	do arithmetic calculations, 
	etc.



Usually, 
	the template files will have the .j2 extension
	denotes the Jinja2 templating engine used.

The double curly braces will denote the variables in a template file, '{{variables}}'.

We need to have two parameters when using the Ansible Template module, such as:
	src: 
		The source of the template file. 
		can be 
			relative or 
			absolute.
	dest: 
		Dest is the destination path on the remote server.

Template Module Attributes
	Here are some other parameters which can be used to change some default behavior of the template module:

Force: 
	If the destination file already exists, 
		then the Force parameter will decide whether it should be replaced or not. 
		By default, the value is yes.
Mode: 
	This parameter is used to set the permissions for the destination file explicitly.
Backup: 
	If you want a backup file to be created in the destination directory, 
		set the value of the backup parameter to yes. 
		By default, the value is no. 
			the backup file will be created every time there is a change in the destination directory.
Group: 
	Name of the group that should own the directory. 
	It is similar to executing chown command for a file in Linux systems.




A practical example of using template 
	https://www.learnlinux.tv/getting-started-with-ansible-16-templates/


mkdir myroles
	cd myroles

ansible-galaxy init mytemplate

tree mytemplate
mytemplate
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── README.md
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml

cat tasks/main.yml
----------------------------------
---
# tasks file for mytemp
- name: copy mytemp file
  template:
     src: templates/mytemp.j2
     dest: /home/vilas
     owner: root
     group: root
     mode: 0444
----------------------------------

cat templates/mytemp.j2
----------------------------------
Welcome to {{ ansible_hostname }}

This file was created on {{ ansible_date_time.date }}
Learning ansible templates {{ myhostvar }}

Contact {{ tempowner }} if anything is wrong
----------------------------------


cat defaults/main.yml
----------------------------------

---
# defaults file for mytemp
tempowner: vilas.varghese@gmail.com
----------------------------------





mkdir host_vars
cat /etc/ansible/host_vars/ubuntu1
----------------------------------
myhostvar: This is ubuntu machine
----------------------------------


cat /etc/ansible/host_vars/cent1
----------------------------------
myhostvar: This is centos1 machine
----------------------------------

cat /etc/ansible/host_vars/cent2
----------------------------------
myhostvar: This is centos2 machine
----------------------------------

cd ..
cat testtemp.yml
----------------------------------
---
- name: Learn templates
  hosts: all
#  user: vilas
  become: true

  roles:
    - role: mytemplate
      tempowner: vilas_varghese@yahoo.com
----------------------------------


ansible-playbook testtemp.yml




###############################################

Alternatives to follow

###############################################


1. Templating simple values
	
	Consider following file structure:
		.
		├── ansible.cfg
		├── templates
		│   └── my_app.conf.j2
		└── simple_playbook.yaml
		
		Refer 
			simple_playbook.yaml
			templates/my_app.conf.j2
		
		
	ansible-plyabook simple_playbook.yaml

	output would be 
	-----------------------------------------------------------------------------------
		Produces the following on "Ubuntu 18.04/Centos 7.5" host: $HOME/my_app.conf

		local_ip = 10.1.11.72
		local_user = ubuntu/centos
	-----------------------------------------------------------------------------------
	

2. Templating some more default values
	.
	├── ansible.cfg
	├── templates
	│   └── special_variables.j2
	└── default_playbook.yaml

	ansible-plyabook default_playbook.yaml

	output would be 
	-----------------------------------------------------------------------------------
	ansible_managed = "Ansible managed"
	template_host = "My host name e.g. myansiblehost"
	template_uid = "vagrant"
	template_path = "/path/to/ansible/templates/special_variables.j2"
	template_fullpath = "/path/to/ansible/templates/special_variables.j2"
	template_run_date = "2020-08-16 15:24:05.185182"
	-----------------------------------------------------------------------------------


3. Template a file onto a remote host
	Refer template_with_execute_access.yaml
	
4. 