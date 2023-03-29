
Refer sampleformat.yaml

git clone https://github.com/vilasvarghese/ansible.git
cd ansible/playbook

----------------------------------------------------------------------------------------------------------------------------
N.B: Default all the below instructions would execute against /etc/ansible/hosts
You can modify it by adding -i <inventory> while executing the command.

1. 	createfile.yaml is a simple playbook which would create a file in /home directory.

------------------------------------------
Below commands expects the default inventory 
/etc/ansible/hosts should be updated
------------------------------------------
	Syntax check
	ansible-playbook <playbook.yml> --check
		ansible-playbook --syntax-check <playbook.yml> 
			Success Output: filename
			Failure Output: error message with description
		
	List hosts
		ansible-playbook <playbook.yml> --list-hosts
	
	Run the playbook
		ansible-playbook createfile.yaml -kK

2. 	Create a directory
		ansible-playbook createdir.yaml -kK
	
3. 	Create multiple directories using a single task.
	This shows how a single instruction can be repeated with multiple parameters.
		ansible-playbook createmultipledir.yaml

4. 	Print the date
		ansible-playbook date.yaml -kK

5. 	Print the time
		ansible-playbook time.yaml -kK
		
6. 	Use of multiple tasks in a playbook
		ansible-playbook datetime.yaml
		ansible-playbook datetimelog.yaml

7. 	Install vim and git
			modify yum to apt in installvimgit.yaml
		ansible-playbook installvimgit.yaml
		Go to the host and verify
		ansible servers -m yum/apt -a "name=git state=absent" -b -kK
		ansible servers -m yum -a "name=vim state=absent" -b

8. 	Install and start apache
		ansible-playbook installandstart.yaml
		Both the instructions below would fail.
		curl <ip:80>
		curl <ip:8080>
		ansible servers -m yum -a "name=httpd state=absent" -b
		ansible servers -m apt -a "name=apache2 state=absent" -b

9. 	This is wip
	Create user
		this would fail since pwd is not encrypted. That is a check from the module.
		ansible-playbook createuser.yml
		
		This can be done using jinja as follows
		ansible-playbook newcreateuser.yml  --extra-vars "uusername=ullas upassword=Ullas@"
		ansible-playbook m.yml  --extra-vars "uusername=vilas upassword=vilas123"
-----------------------------------------------------------------
cd orchestration
-----------------------------------------------------------------	
10. 	Install and start apache
		ansible-playbook apache.yaml
		Now the curl should succeed.
		curl <ip:80>
		

		Go to the host 
		systemctl status httpd
		ansible servers -m shell -a "systemctl status httpd" -b
		ansible servers -m yum -a "name=httpd state=absent" -b

11. 	pre_tasks and post_tasks
		A real work usecase for reference.
			https://www.middlewareinventory.com/blog/ansible-pre-tasks-and-post-tasks-example/
			
			