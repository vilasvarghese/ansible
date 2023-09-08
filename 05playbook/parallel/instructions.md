Change the number of forks
--------------------------
1. Update forks in ansible.cfg
	Default: /etc/ansible/ansible.cfg
	Can be overriden by: ~/ansible.cfg
2. Pass along with command line argument
	ansible-playbook -f 10 playbook.yml
	ansible -f 10 -m ping all


Serial
------
Make a playbook execute sequentially 
	Set serial to 1
	ansible-playbook serial.yaml

	Increase the serial count and verify that now parallel execution is happening.