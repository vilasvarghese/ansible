In the playbook
	generally variables are defined as ‘variable_name: variable_value‘ format. 
	Use variable_name inside double braces 
	
for e.g.
	
1. ansible-playbook hello.yaml

2. variables can be also array or list type. 
	Refer: country.yaml

hello contains 9 values, and each one can be accessed using the index numbers(starting from zer0). The following task will output ‘South America‘

