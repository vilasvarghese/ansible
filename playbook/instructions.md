
Refer sampleformat.yaml

git clone https://github.com/vilasvarghese/ansible.git
cd ansible/playbook

1. 	createfile.yaml is a simple playbook which would create a file in /home directory.

	Syntax check
		ansible-playbook <playbook.yml> --syntax-check

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

7. 	