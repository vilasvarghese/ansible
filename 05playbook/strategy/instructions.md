With reference to inventory.yaml in the current directory.

1. Modifying the number of batch size
---------------------------------------
	Added serial: 2 in the playbook
---------------------------------------	
	ansible-play -i inventory.yaml serial_playbook.yaml

	Ansible would execute the play completely (both tasks) on 
			2 of the hosts 
			before moving on to the next 2 hosts:
		Watch out for the execution logs.
		
	
2. 	Mention the batch size in percentage
---------------------------------------
	serial: "30%"
---------------------------------------
	Ansible applies the percentage to 
		the total number of hosts in a play 
		to determine the number of hosts per pass:
		
	ansible-play -i inventory.yaml percentage_playbook.yaml
	

3. 	Mention batch sizes as a list
---------------------------------------
	  serial:
		- 1
		- 2
		- 5
---------------------------------------
	
	ansible-play -i inventory.yaml list_playbook.yaml
		In list_playbook.yaml, 
			first batch contain a single host, 
			next would contain 2 hosts, 
			and (if there are any hosts left), 
			every following batch would contain 
				either 5 hosts or all the remaining hosts, 
					if fewer than 5 hosts remained.
	
4. 	We can have a list of percentage
	ansible-play -i inventory.yaml list_percentage_playbook.yaml
---------------------------------------
	  serial:
		- "10%"
		- "20%"
		- "100%"
---------------------------------------
	
5. 	A combination of percentage and count combination is also supported
	ansible-play -i inventory.yaml combination_playbook.yaml
---------------------------------------

  serial:
    - 1
    - 5
    - "20%"
---------------------------------------

	
6. Debug strategy
		Execute anyplaybook that fail
		while it fails it waits for us to debug
		
	dir()
	task_vars.keys()
	task_vars.get("ansible_connection")
	 exit()
---------------------------------------
---------------------------------------
	 
	 
	 