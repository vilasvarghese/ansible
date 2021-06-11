ansible -i inventory localhost -m shell -a "free -m"

General syntax
--------------
ansible [-i <inventory>] host:group1:group2 -m module [-a arguments]

1. Ping command
	ansible <group> -m ping
	e.g.
		ansible all -m ping

2. Execute any shell command 
	a. Execute shell command "uptime" on a particular host.
	ansible <ip> -m shell -a "uptime"
	ansible <ip> -m shell -a "free -m"
	#-m: module
	#-a: argument

	b. Execute shell command "uptime" on multiple groups.
	ansible <group1>:<group2> -m shell -a "uptime"


3. Find the list of modules
	List all modules
		ansible-doc -l

4. Number of default modules
	ansible-doc -l | wc -l
	
5. Get Help on a module
	ansible-doc module
	e.g. ansible-doc shell
	
6. See the setup. This will show the hardware details of the box.
	a. See host hardware details 
		ansible host2 -m setup
		#host definition in inventory file: host2 ansible_ssh_host=192.168.176.143
	
	b. See group hardware details
		ansible servers -m setup
		#servers is a groupname [servers] in inventory file.
		
	c. Filter the ansible details from the setup.
		ansible host2 -m setup -a "filter=ansible_distribution*"
		
7. 	Execute fdisk as vagrant with sudo user
	ansible servers -m shell -a 'fdisk -l' -u vagrant --become -K
	#become sudo user
	#-K: ask for pwd

8. 	Modifying default 
	-----------------
	Use adhoc commands for this.
		
	a.	Ansible uses 5 simultaneous processes (nodes). 
	
		Reboot the [servers] servers with 10 parallel forks:
		$ ansible servers -a "/sbin/reboot" -f 10

	b. /usr/bin/ansible will default to running from your user account. 
	
		Connect as a different user:
		$ ansible servers -a "/sbin/reboot" -f 10 -u username


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
	
11. Install nginx
	a. using apt
		ansible servers -m apt -a 'name=nginx state=latest' --become
		
	b. using yum
		ansible servers -m yum -a "name=nginx state=present" --become
		If sudo privileges are asked, then use 
		ansible servers -m yum -a "name=nginx state=present" -b -kK
	
12. Start nginx 
	ansible servers -m service -a 'name=nginx state=started enabled=yes' --become
	

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