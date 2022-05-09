1. Single handler
	ansible-playbook nginx_handler.yaml
	ansible-playbook ubuntu_nginx_handler.yaml

2. Multiple handler
	ansible-playbook multiple_handler.yaml
	
Handlers are just like normal tasks in an Ansible playbook but they run only when if the Task contains a “notify” directive. It also indicates that it changed something.

A handler is called at the end of playbook by default
	so even if a handler is notified multiple times
		the tasks under handler will only run once. 
	Handlers are executed ONLY when one of the calling tasks have state changed.
		If state of a task is not changed, then attached notify will not call handler.	
	Handlers will always run in same order in which handlers are defined 
	Handlers don’t run in the order of notify. 


Name of a handler should be unique
	This is used for calling it by notifier.
If two handlers have the same name, only the one will run. 
	Which is defined later in playbook.

You can mention a variable in handler’s tasks section. 
	But avoid using variables in name of a handler. 
	As handler names are templated early on.
