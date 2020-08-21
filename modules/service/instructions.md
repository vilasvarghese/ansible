
cd ansible/modules/service

1. Start a Service
	
	a. ad-hoc
		ansible all -m service -a "name=httpd state=started"
	b. playbook
		ansible-playbook start_service.yaml

2. Stop a Service

	a. ad-hoc
	ansible all -m service -a "name=crond state=stopped"

	b. playbook
	ansible-playbook stop_service.yaml


3. Restart Service


	a. ad-hoc
		ansible all -m service -a "name=httpd state=restarted"

	b. playbook
		ansible-playbook restart_service.yaml

4. Start and Enable Service

	a. ad-hoc
		ansible all -m service -a "name=httpd state=started enabled=yes"

	b. playbook
		ansible-playbook start_enable_service.yaml

5. Working on Multiple Services in Single Playbook


		ansible-playbook reload_multiple_service.yaml


6. Working Differently on Different Servers
	ansible-playbook diff_service_diff_server.yaml

7. Capturing the Output & Display
	ansible-playbook service_operation_status.yaml

