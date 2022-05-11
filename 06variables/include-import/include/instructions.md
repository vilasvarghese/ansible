#################################################################
# Not fully cooked to work. But explains the concept well
#################################################################

include_tasks module 
	include tasks from a file into another list of tasks. 
	This could be within a playbook or a role’s task files. 
	
	#all the below should be executed from with in a role.

1.	
	Refer 
		install.yaml and 
		configure.yaml
		
	This can be called from 
		main.yaml
		
	
2. 	Refer
	conditional-include.yaml
	
3. 	