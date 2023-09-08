#Create a relevant inventory file 

0. Simple playbook duplicate and repeat a lot. Look the below code.

	ansible-playbook -i inventory.yaml no_loop.yaml  -K
	ansible-playbook no_loop.yaml  -K

	The same playbook can be written using the loop directive. 
	The loop directive executes the same task multiple times. 
	It  stores the value of each item in a variable called item. 
	So, instead of specifying the names of the users to be added, 
		specify a variable called item 
		enclosed between double curly braces as shown.

	e.g.
		name: ‘{{ item }}’

1. Let's create the same set of user's in both ways.
	Using loop
	----------
		ansible-playbook -i inventory.yaml  create_user_in_loop.yaml -K
		ansible-playbook create_user_in_loop.yaml -K
	
	Using with_*
	------------
		ansible-playbook  -i inventory.yaml create_user_using_with.yaml
		ansible-playbook  create_user_using_with.yaml -K
	
	loop is relatively new entry and both are similar
	with_ keywords rely on Lookup Plugins - even items is a lookup.
	
2.  Iterating over a list of Dictionaries
	
	Above we looked at a simple standard loop 
		the array was a list of string values 
		representing users to be added to the remote target.

	What if we need to add the uid to the loop such that each item now has two values: 
		username and 
		uid. 
	How would you pass 2 values in an array?

		pass an array of dictionaries, 
		each with 2 key-value pairs. 
		keys: name and 
		uid: values 
			will be the username and the ID of each user.

	-----------------------------------------------------------
		ansible-playbook  -i inventory.yaml create_user_using_dictionary.yaml
	-----------------------------------------------------------
	
		Under the ‘tasks‘ section, Since we have 2 values, it will translate into two variables: 
			item.name & 
			item.uid.
		
		Items can be represented as json as below..
-------------------------------------------------------------------
loop:
  - { name: john , uid: 1020 }
  - { name: mike , uid: 1030 }
  - { name: andrew , uid: 1040}		
-------------------------------------------------------------------

3. Ansible Loops with indexes
	Sometimes, we need to track index of items. 
	For this, 
		use the ‘with indexed_items‘ lookup. 
		The loop index begins from item.0 and the value from item.1

	a) ansible-playbook -i inventory.yaml loop_with_index.yaml
		ansible-playbook loop_with_index.yaml

	b) Compare create_user_using_dictionary.yaml and loop_with_index.yaml

	c) ansible-playbook loop_with_index_playbook.yaml
	

4. We can add to index 
	ansible-playbook loop_index_add_playbook.yaml
	Compare loop_index_add_playbook.yaml and loop_with_index_playbook.yaml

5. Conditionals in Ansible Loops
	
	Loops you can use the conditional statement 
		to control the looping 
		based on 
			variables or 
			system facts. 
	
-----------------------------------------------------------	
	ansible-playbook conditional_loops.yaml
-----------------------------------------------------------

	
	We have specified an array called ‘packages‘ 
		that contains a list of packages 
		that need to be installed. 
		
	Each of the items on the array contains 
		name of the package to be installed and 
		property called ‘required‘ which is set to ‘True‘ 
		for 2 packages and ‘False‘ for one package.

	Here we are passing the list of software to install as a variable though loop.


6. Loop with dictionaries
	ansible-playbook loop_with_dictionary.yaml