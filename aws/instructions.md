rhel
ssh -vvv -i "vilasaws.pem" ec2-user@ec2-3-21-241-17.us-east-2.compute.amazonaws.com

centos
ssh -vvv -i "vilasaws.pem" centos@ec2-3-21-241-17.us-east-2.compute.amazonaws.com


######################################################################################################
Create a new user on the controller with same name

	On the EC2 instance
	
	1. Create a new user 
		CentOS
			sudo su
			adduser vilas
			passwd vilas
		
	2. Add the user to sudo privilege
		usermod -aG wheel vilas
		su - vilas

	3. Add ssh support
		mkdir .ssh
		cd .ssh
		vi authorized_keys
--------------------------------------------------------------
		Paste the public key generated from the control node here.
			
		ssh-keygen -t rsa -C vilas@192.168.176.142
	
		cd /home/vilas/.ssh
		ls -a
		
		ssh-copy-id vilas@<ip>
	

Set the following in ansible.cfg file
------------------------------------------------------------------------
[defaults]
inventory = ./hosts-dev
remote_user = <SSH_USERNAME>
private_key_file = ~/.ssh/id_rsa
host_key_checking = False
------------------------------------------------------------------------	
			
--------------------------------------------------------------
		cd /etc/ssh
		vi ssh_config
--------------------------------------------------------------		
			PasswordAuthentication yes
--------------------------------------------------------------
		vi sshd_config
--------------------------------------------------------------		
			PasswordAuthentication yes
--------------------------------------------------------------
		systemctl restart sshd
			
		ansible -i hosts -m ping aws
		
#########################################################################################

Dynamic inventory
		
	1. 	Go to the link below 
	https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#intro-dynamic-inventory
	
	2. Click on "inventory scripts" link which takes user to 
	https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory
	
	3. Open 
		ec2.ini
		ec2.py
		
		Copy paste it to the control node.

	Install Ansible EC2 module dependencies
		CentOS
			yum install python-pip
			pip install --upgrade pip

		Ubuntu
			sudo apt install python
			sudo apt install python-pip
		
		RHEL (On EPEL 6 and EPEL7)
			Same as CentOS
			Otherwise refer 
				https://packaging.python.org/guides/installing-using-linux-tools/

-----------------------------------------				
		pip install boto boto3 
		#default code can work with both boto and boto3. Priority being for boto.
		#Preferably have both.
-----------------------------------------
		
	4. Need to add roles for the ec2 instances.
		Error you get if the role is not properly set.
			"Check your credentials"
			
		Create a role with "AmazonEc2FullAccess" permission policy.
		Attach the instance to that role.
		
		export AWS_ACCESS_KEY_ID='changed'
		Click on "Create Access Key" and paste it below
		export AWS_SECRET_ACCESS_KEY='<key>'
		export ANSIBLE_HOSTS=/etc/ansible/ec2.py
		export EC2_INI_PATH=/etc/ansible/ec2.ini
######################################################################################################
Steps to connect
	Create AWS user
	Install Ansible and Ansible EC2 module dependencies
	Create SSH keys
	Create Ansible structure
	Run Ansible to provision the EC2 instance
	Connect to the EC2 instance via SSH
######################################################################################################	

######################################################################################################
	Create AWS user
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console

	1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

	2. In the navigation pane, 
		choose Users and then 
		choose Add user.
		Type the user name for the new user. 
			This is the sign-in name for AWS. 
			Keep this same as the user you may use on-prem.
			
		N.B: User names can be a combination of up to 64 letters, digits, and these characters: plus (+), equal (=), comma (,), period (.), at sign (@), underscore (_), and hyphen (-). Names must be unique within an account. They are not distinguished by case. For example, you cannot create two users named TESTUSER and testuser.

		Select the type of access: AWS Management Console access
		Console password: Custom password
						Enter the password of your choice
		Unset: Require password reset

		Click "Next: Permissions"

	3. On the Set permissions page, Choose one of the following three options:
		Add user to group. 
		(Optional) Set a permissions boundary. This is an advanced feature.
			Create user without a permissions boundary
			
		Click Next: Tags.

	4. In Add tags (optional)
		Enter 
			Key: Permissions
			Value: All
	5. Click Next: Review 
	
	6. When you are ready to proceed, choose Create user.

######################################################################################################
	Install Ansible EC2 module dependencies
		CentOS
			yum install python-pip
			pip install --upgrade pip

		Ubuntu
			sudo apt install python
			sudo apt install python-pip
		
		RHEL (On EPEL 6 and EPEL7)
			Same as CentOS
			Otherwise refer 
				https://packaging.python.org/guides/installing-using-linux-tools/

-----------------------------------------				
		pip install boto boto3
-----------------------------------------
for installing ansible
-----------------------------------------
		pip install ansible
-----------------------------------------
######################################################################################################
	Create SSH keys

######################################################################################################
	Create Ansible structure

######################################################################################################
	Run Ansible to provision the EC2 instance

######################################################################################################
	Connect to the EC2 instance via SSH
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################
######################################################################################################

Create a role
	https://console.aws.amazon.com/iam/home#/roles

https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/



