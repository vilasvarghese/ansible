1. Ignore if there are errors.

2. #Resetting unreachable hosts
If Ansible cannot connect to a host, 
	it marks that host as ‘UNREACHABLE’ 
	removes it from the list of active hosts for the run. 
	You can use meta: 
		clear_host_errors to reactivate all hosts, 
		so subsequent tasks can try to reach them again.

3. Handlers and failure
	Ansible runs handlers at the end of each play. 
	If a task notifies a handler but another task fails later in the play, 
		by default the handler does not run on that host, 
		which may leave the host in an unexpected state. 
	If required change this behavior with the 
		--force-handlers command-line option
			includ force_handlers: True in a play, 
			or by adding force_handlers = True to ansible.cfg. 
	When handlers are forced, 
		Ansible will run all notified handlers on all hosts, 
		even hosts with failed tasks. 
		(Note that certain errors could still prevent the handler from running
			e.g. 
			host becoming unreachable.)

4. Defining failure
	Ansible lets you define what “failure” means in each task 
		use failed_when conditional. 
	As with all conditionals in Ansible, 
		lists of multiple failed_when conditions are joined with an implicit and, 
			meaning the task only fails when all conditions are met. 
	If you want to trigger a failure when any of the conditions is met, 
		define conditions in a string with an explicit or operator.

	e.g.

		- name: Fail task when the command error output prints FAILED
		  ansible.builtin.command: /usr/bin/example-command -x -y -z
		  register: command_result
		  failed_when: "'FAILED' in command_result.stderr"

		or based on the return code
		- name: Fail task when both files are identical
		  ansible.builtin.raw: diff foo/file1 bar/file2
		  register: diff_cmd
		  failed_when: diff_cmd.rc == 0 or diff_cmd.rc >= 2

		or implicit and
		- name: Check if a file exists in temp and fail task if it does
		  ansible.builtin.command: ls /tmp/this_should_not_be_here
		  register: result
		  failed_when:
			- result.rc == 0
			- '"No such" not in result.stdout'
		i.e. result.rc == 0 and '"No such" not in result.stdout'

		or explicit or
		failed_when: result.rc == 0 or "No such" not in result.stdout


		or split conditions to multi-line YAML value with >.

		- name: example of many failed_when conditions with OR
		  ansible.builtin.shell: "./myBinary"
		  register: ret
		  failed_when: >
			("No such file or directory" in ret.stdout) or
			(ret.stderr != '') or
			(ret.rc == 10)



5. Defining “changed”
	Ansible lets you define when a particular task has “changed” a remote node 
		use the changed_when conditional. 
	This lets you determine, 
		based on return codes or output, 
		whether a change should be reported in Ansible statistics and 
		whether a handler should be triggered or not. 
	As with all conditionals in Ansible, 
		lists of multiple changed_when conditions are joined with an implicit and, 
		meaning the task only reports a change when all conditions are met. 
	To override define the conditions in a string with an explicit or operator. 
For example:

	tasks:
	  - name: Report 'changed' when the return code is not equal to 2
		ansible.builtin.shell: /usr/bin/billybass --mode="take me to the river"
		register: bass_result
		changed_when: "bass_result.rc != 2"

	  - name: This will never report 'changed' status
		ansible.builtin.shell: wall 'beep'
		changed_when: False

	You can also combine multiple conditions to override “changed” result.
	- name: Combine multiple conditions to override 'changed' result
	  ansible.builtin.command: /bin/fake_command
	  register: result
	  ignore_errors: True
	  changed_when:
		- '"ERROR" in result.stderr'
		- result.rc == 2
		
		
6. Ensuring success for command and shell	
	Success/failure of a command and shell is determined by it's return codes
	
tasks:
  - name: Run this command and ignore the result
    ansible.builtin.shell: /usr/bin/somecommand || /bin/true
	
	
7. Aborting a play on all hosts
	You may want to abort a play
		first failure on a single host
	or 
		failures on a certain percentage of hosts
	
	To stop play execution after the first failure 
		use any_errors_fatal. 
	For finer-grained control, 
		use max_fail_percentage to abort 
			after a given percentage of hosts has failed	

-----------------------------------------------	
- hosts: somehosts
  any_errors_fatal: true
  roles:
    - myrole

- hosts: somehosts
  tasks:
    - block:
        - include_tasks: mytasks.yml
      any_errors_fatal: true
-----------------------------------------------
-----------------------------------------------
---
- hosts: load_balancers_dc_a
  any_errors_fatal: true

  tasks:
    - name: Shut down datacenter 'A'
      ansible.builtin.command: /usr/bin/disable-dc

- hosts: frontends_dc_a

  tasks:
    - name: Stop service
      ansible.builtin.command: /usr/bin/stop-software

    - name: Update software
      ansible.builtin.command: /usr/bin/upgrade-software

- hosts: load_balancers_dc_a

  tasks:
    - name: Start datacenter 'A'
      ansible.builtin.command: /usr/bin/enable-dc

-----------------------------------------------  
	
---
- hosts: webservers
  max_fail_percentage: 30
  serial: 10
  
  
In a block we can resue

-----------------------------------------------  
 tasks:
 - name: Handle the error
   block:
     - name: Print a message
       ansible.builtin.debug:
         msg: 'I execute normally'

     - name: Force a failure
       ansible.builtin.command: /bin/false

     - name: Never print this
       ansible.builtin.debug:
         msg: 'I never execute, due to the above task failing, :-('
   rescue:
     - name: Print when errors
       ansible.builtin.debug:
         msg: 'I caught an error, can do stuff here to fix it, :-)'  
-----------------------------------------------  		 