
Refer sampleformat.yaml

git clone https://github.com/vilasvarghese/ansible.git
cd ansible/playbook

----------------------------------------------------------------------------------------------------------------------------
N.B: Default all the below instructions would execute against /etc/ansible/hosts
You can modify it by adding -i <inventory> while executing the command.

1. 	createfile.yaml is a simple playbook which would create a file in /home directory.

	Syntax check
		ansible-playbook <playbook.yml> --syntax-check
			Success Output: filename
			Failure Output: error message with description
		
	List hosts
		ansible-playbook <playbook.yml> --list-hosts
	
	Run the playbook
		ansible-playbook createfile.yaml

2. 	Create a directory
		ansible-playbook createdir.yaml
	
3. 	Create multiple directories using a single task.
	This shows how a single instruction can be repeated with multiple parameters.
		ansible-playbook createmultipledir.yaml

4. 	Print the date
		ansible-playbook date.yaml

5. 	Print the time
		ansible-playbook time.yaml
		
6. 	Use of multiple tasks in a playbook
		ansible-playbook datetime.yaml
		ansible-playbook datetimelog.yaml

7. 	Install vim and git
		ansible-playbook installvimgit.yaml
		Go to the host and verify
		ansible servers -m yum -a "name=git state=absent" -b
		ansible servers -m yum -a "name=vim state=absent" -b

8. 	Install and start apache
		ansible-playbook installandstart.yaml
		Both the instructions below would fail.
		curl <ip:80>
		curl <ip:8080>
		ansible servers -m yum -a "name=httpd state=absent" -b
-----------------------------------------------------------------
cd orchestration
-----------------------------------------------------------------	
9. 	Install and start apache
		ansible-playbook apache.yaml
		Now the curl should succeed.
		curl <ip:80>
		

		Go to the host 
		systemctl status httpd
		ansible servers -m shell -a "systemctl status httpd" -b
		ansible servers -m yum -a "name=httpd state=absent" -b

9. 	Download and install java
		