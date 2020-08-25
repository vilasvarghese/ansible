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