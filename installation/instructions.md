Installing Ansible on RHEL/CentOS
---------------------------------

	RHEL specific
	-------------
	RPMs for RHEL 7 and RHEL 8 are available from the Ansible Engine repository.
	Enable the Ansible Engine repository for RHEL 8
		$ sudo subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms

	Enable the Ansible Engine repository for RHEL 7, run the following command:
		$ sudo subscription-manager repos --enable rhel-7-server-ansible-2.9-rpms



	On RHEL and CentOS:
	$ sudo yum -y install epel-release
	$ sudo yum -y install python-argcomplete
	$ sudo yum -y install ansible			
		
		
	Ubuntu specific
	---------------
	$ sudo apt -y update
	$ sudo apt install -y software-properties-common
	$ sudo apt-add-repository --yes --update ppa:ansible/ansible
	$ sudo apt install -y ansible



Detailed reference:	https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements
Other good references
https://ansible-tips-and-tricks.readthedocs.io/en/latest/ansible/install/


	2. Installing Ansible with pip
	--------------------------
	Ansible can be installed with pip, 
	the Python package manager. 

	1. sudo su
	

	2. 	install required dependencies for pip install.
	yum install python-setuptools
	
	3. 
	If pip is not available execute:

	sudo easy_install pip

	or 
	$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
	$ python get-pip.py --user
	

	4. sudo pip install ansible
	
	5. Create ansible.cfg and hosts file - copy from my repo.
	
	Installing ansible on ubuntu using pip
	--------------------------------------
	https://www.toptechskills.com/ansible-tutorials-courses/how-to-install-ansible-ubuntu-linux-1804-bionic-tutorial/
	
	
	
	
	
	
		Then install Ansible:
		$ pip install --user ansible

	Create ansible.cfg, hosts etc from the github.
	
	
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
	ssh-keygen -t rsa -C root@192.168.176.142
	
	cd /root/.ssh
	ls -a
	
	ssh-copy-id vagrant@192.168.176.143
	ssh-copy-id vagrant@192.168.176.144
	

Set the following in ansible.cfg file
------------------------------------------------------------------------
[defaults]
inventory = ./hosts-dev
remote_user = <SSH_USERNAME>
private_key_file = ~/.ssh/id_rsa
host_key_checking = False
------------------------------------------------------------------------	
	
	sudo chmod 444 ~/.ssh/id_rsa
	
Additional
----------
https://docs.ansible.com/ansible/latest/user_guide/connection_details.html
