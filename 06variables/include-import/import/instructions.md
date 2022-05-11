#######################################################################################
#	This is not fully cooked. It explains the concept well.
#######################################################################################

1. We can define common roles as common_roles.yaml

2. We can import it into another playbook as in
	web.yaml

3. Reuse the same playbook again as in
	database.yaml

Advantage
	a. Much more cleaner code.
	b. No repetition.
	c. Easy to maintain.
		Any modificatin to common roles can be managed easily.

4. Execuet web.yaml as 
	$ ansible-playbook web.yaml --extra-vars "server_type=web"

5. Execute database.yaml
	$ ansible-playbook database.yaml --extra-vars "server_type=database"


	Import a playbook conditionally
	-------------------------------

1. Create a staging specific role.
	staging_roles.yaml

2. Conditionally import as in
	web-staging.yaml

3. Conditionally import as in
	database-staging.yaml

	env variable were added to limit the servers the playbooks configure

4. 	Execute web-staging.yaml with env. variables being passed as below
	$ ansible-playbook web-staging.yaml --extra-vars "env=staging server_type=web"

5. 	Execute database-staging.yaml with env. variables being passed as below
	$ ansible-playbook database-staging.yaml --extra-vars "env=staging server_type=database"


	Import playbook with dynamic filename
	-------------------------------------
Refer to 
	master-dynamic.yaml, 
	web-dynamic.yaml
	db-dynamic.yaml

Invoke it as follows
	$ ansible-playbook master-dynamic.yaml --extra-vars "env=staging server_type=web-dynamic"
	$ ansible-playbook master-dynamic.yaml --extra-vars "env=staging server_type=db-dynamic"