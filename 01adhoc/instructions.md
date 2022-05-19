ansible -i inventory localhost -m shell -a "free -m"

General syntax
--------------
ansible [-i <inventory>] host:group1:group2 -m module [-a arguments]

1. Ping command
	ansible -i <inventory file location> <group> -m ping
	e.g. 
		ansible -i inventory.yml centos -m ping
------------------------------------------------------------------------------------------------
[win]
#localhost              ansible_connection=local
172.31.11.242           ansible_connection=winrm       ansible_user=Administrator               ansible_password=<my pwd> ansible_winrm_transport=basic ansible_winrm_server_cert_validation=ignore
#other2.example.com     ansible_connection=ssh        ansible_user=myotheruser

[centos]
172.31.23.28            ansible_user=vilas              ansible_password=<my pwd>
--------------------------------------------------------------------------------------------------------------------------------
	This command is making a connection to the clients and not a simple ping.


2. echo "testing" >> abc.txt

ansible -i inventory.yml centos -m copy -a "src=abc.txt dest=/tmp/"


3. ansible -i inventory.yml centos -a "cat /tmp/abc.txt" 

4. without root
		yum install git
		sudo yum install git
		sudo yum remove git

	Install git
	ansible -i inventory.yml centos -m yum -a "name=git state=present" --become
	ansible -i inventory.yml ubuntu -m apt -a "name=git state=present" --become -kK
		#-k, --ask-pass: ask for connection password #ignore if mentioned in inventory
		#-K, --ask-become-pass: ask for privilege escalation password
	
		on the centos machine
			git 
	
	 ansible -i inventory.yml centos -b -m git -a "repo=https://github.com/vilasvarghese/ansible.git dest=/home/centos/tmp version=HEAD"
	
		remove it from centos box
			sudo rm -rf tmp/
	
5. Remove git
	ansible -i inventory.yml centos -m yum -a "name=git state=removed" --become
		on the centos machine
			git 

6. Create an ansible.cfg
----------------------------------------------
[defaults]
inventory = inventory.yml
----------------------------------------------
	
	
	ansible centos -m ping
	
	
7. 	ansible all --list-hosts

8. ansible all -m gather_facts
	ansible all -m gather_facts --limit <ip>
	ansible all -i inventory.yml -m gather_facts

9. Execute any shell command 
	a. Execute shell command "uptime" on a particular host.
		Identify how long the machine is up.
	ansible centos -m shell -a "uptime"
	ansible centos -m shell -a "free -m"
	#-m: module
	#-a: argument

	b. Execute shell command "uptime" on multiple groups.
	ansible <group1>:<group2> -m shell -a "uptime"


10. Find the list of modules
	List all modules
		ansible-doc -l

11. Number of default modules
	ansible-doc -l | wc -l
	
12. Get Help on a module
	ansible-doc module
	e.g. ansible-doc shell
	
13. See the setup. This will show the hardware details of the box.
	a. See host hardware details 
		ansible centos -i inventory.yml -m setup
		ansible ubuntu -i inventory.yml -m setup
		#host definition in inventory file: host2 ansible_ssh_host=192.168.176.143
	
	b. See group hardware details
		ansible centos -m setup
		#servers is a groupname [servers] in inventory file.
		
	c. Filter the ansible details from the setup.
		ansible centos -m setup -a "filter=ansible_distribution*"
		
7. 	Execute fdisk as vagrant with sudo user
	ansible centos -m shell -a 'fdisk -l' -u root --become -K
	#become sudo user
	#-K: ask for pwd
		(enter an incorrect pwd and it may still work if the command doesn't need pwd. like the above)

8. 	Modifying default 
	-----------------
	Use adhoc commands for this.
		
	a.	Ansible uses 5 simultaneous processes (forks - nodes in this case). 
	
		Reboot the [servers] servers with 10 parallel forks:
		$ ansible centos -a "/sbin/reboot" -f 10 --become 

	b. /usr/bin/ansible will default to running from your user account. 
	
		Connect as a different user:
		$ ansible servers -a "/sbin/reboot" -f 10 -u username --become 


9. 	Working with files
	------------------
	Managing files
	Through Ansible and SCP 
		
		a. transfer many files to multiple machines in parallel. 
		
			To transfer a file directly to all servers in the [servers] group:
				$ ansible servers -m copy -a "src=/etc/hosts dest=/tmp/hosts"

			To repeat the above: use template in playbook.

		
		b. change ownership and permissions on files. 
			$ ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"
			$ ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=vilas group=dev"

10. Get a file from remote machine.(e.g. logfile)
	ansible node1 -m fetch -a 'src=/etc/abc/xyz.log dest=/home/vilas/example/ flat=yes'




Other useful commands
---------------------
	ansible servers -m shell -a 'df -h /dev/sda2' --become
	ansible servers -m shell -a 'systemctl status sshd' --become


Installing software using yum/apt commands.
	ansible <group> -m yum -a "name=git state=present" --become
	ansible servers -m yum -a "name=httpd state=latest" -b
	
	ansible <host as mentioned in inventory>  -m yum -a "name=httpd state=latest" -b
	e.g.
	ansible servers -m yum -a "name=httpd state=latest" -b
	
	name=any software which yum supports installation.
	state
		present: install/ensure installed.
		absent: remove
		installed: 
		latest: upgrade to latest.
		removed: 
		got: 
		Vilas: anyother version number: upgrade to that version.
		
		
	A good reference: https://www.middlewareinventory.com/blog/ansible-ad-hoc-commands/	