In the playbook
	generally variables are defined as ‘variable_name: variable_value‘ format. 
	Use variable_name inside double braces 
	
for e.g.
	
1. ansible-playbook 1simple_variable1.yaml
	ansible-playbook 1simple_variable.yaml


	ansible-playbook 2list.yml

	ansible-playbook 3dictionary.yml

	ansible-playbook 4register_output.yml -kK

2. variables can be also array or list type. 
	Refer: 2list.yml

	hello contains 9 values, and each one can be accessed using the index numbers(starting from zer0). 

3. 	