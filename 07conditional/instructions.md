https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html

	In a playbook
		conditionally execute different tasks
			depending on 
				fact 
				variable, 
				result of a previous task. 
		or set a variables 
			depend on the value of other variables. 
		Or create additional groups of hosts 
			based on whether the hosts match certain criteria. 
	Ansible uses Jinja2 tests and filters in conditionals. 
	
	Simple conditional
	------------------
The simplest conditional statement applies to a single task. 
Create the task
	with when statement 
		applies a test. 
The when clause is a raw Jinja2 expression without double curly braces 
When you run the task or playbook, 
	Ansible evaluates the test for all hosts. 
	On any host where the test passes (returns a value of True), 
		Ansible runs that task
For example, 
	if you are installing mysql on multiple machines, 
		some of which have SELinux enabled, 
			you might have a task to configure SELinux to allow mysql to run. 
		You would only want that task to run on machines that have SELinux enabled:

---------------------------------------------------------------------------
tasks:
  - name: Configure SELinux to start mysql on any port
    ansible.posix.seboolean:
      name: mysql_connect_any
      state: true
      persistent: yes
    when: ansible_selinux.status == "enabled"
---------------------------------------------------------------------------
    # all variables can be used directly in conditionals without double curly braces
	# The value of ansible_selinux.status can be passed from inventory.
	
	Conditional with facts
	----------------------

Execute or skip a task based on facts. 
e.g.	
	You can install a certain package only when the operating system is a particular version.
	You can skip configuring a firewall on hosts with internal IP addresses.
	You can perform cleanup tasks only when a filesystem is getting full.	
---------------------------------------------------------------------------	
tasks:
  - name: Shut down Debian flavored systems
    ansible.builtin.command: /sbin/shutdown -t now
    when: ansible_facts['os_family'] == "Debian"
---------------------------------------------------------------------------	
	List of commonly used facts: https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html#commonly-used-facts
	
	multiple conditions
	-------------------
	group them with parentheses

---------------------------------------------------------------------------	
tasks:
  - name: Shut down CentOS 6 and Debian 7 systems
    ansible.builtin.command: /sbin/shutdown -t now
    when: (ansible_facts['distribution'] == "CentOS" and ansible_facts['distribution_major_version'] == "6") or
          (ansible_facts['distribution'] == "Debian" and ansible_facts['distribution_major_version'] == "7")
---------------------------------------------------------------------------

	logical operators
	-----------------
use logical operators to combine conditions

If you have multiple conditions that all need to be true 
	(that is, a logical "and")
	then just list them (no operator required)

---------------------------------------------------------------------------	
tasks:
  - name: Shut down CentOS 6 systems
    ansible.builtin.command: /sbin/shutdown -t now
    when:
      - ansible_facts['distribution'] == "CentOS"
      - ansible_facts['distribution_major_version'] == "6"
---------------------------------------------------------------------------
	  
	If 
		a fact or variable is a string
	or	
		need to run a mathematical comparison 
	or
		value is an integer
	then use logical operator "and"/"or" etc.	
		
tasks:
  - ansible.builtin.shell: echo "only on Red Hat 6, derivatives, and later"
    when: ansible_facts['os_family'] == "RedHat" and ansible_facts['lsb']['major_release'] | int >= 6


		
		
Refer below for a list of various option jinja2 supports.
	https://jinja.palletsprojects.com/en/latest/templates

	Logic operators supported are 
		and
			Return true if the left and the right operand are true.

		or
			Return true if the left or the right operand are true.

		not
			negate a statement (see below).

		(expr)
			Parentheses group an expression.	
		
		
	Comparisons Operator
		==
			Compares two objects for equality.

		!=
			Compares two objects for inequality.

		>
			true if the left hand side is greater than the right hand side.

		>=
			true if the left hand side is greater or equal to the right hand side.

		<
			true if the left hand side is lower than the right hand side.

		<=
			true if the left hand side is lower or equal to the right hand side.	
		
		

	Conditions based on registered variables
	----------------------------------------
C:\PraiseTheLord\HSBGInfotech\Others\vilas\ansible\07conditional\4condition-registered-var.yml
	
	If the variable is not a list,
	----------------------------------------

		convert it into a list using 
			stdout_lines or
			variable.stdout.split()
		
C:\PraiseTheLord\HSBGInfotech\Others\vilas\ansible\07conditional\5registered-condition-convert-string-to-list.yml	
	
	Check Empty
	----------------------------------------

	C:\PraiseTheLord\HSBGInfotech\Others\vilas\ansible\07conditional\6check-empty.yml

	If you want to execute a task based on success or failure of a task
	--------------------------------------------------------------------------------
	Add ignore_errors: true to the task for which you want to act based on result
		otherwise task will fail if there is an error

	Add follow-up tasks on condition on registered variable like 
		when: <registered variable > is failed
		when: <registered variable > is succeeded
		when: <registered variable > is skipped

	C:\PraiseTheLord\HSBGInfotech\Others\vilas\ansible\07conditional\7execute-on-condition.yml
	
	Using conditionals in loops
	-----------------------------
	8-condition-in-loop
	
	
	More conditionals refer
	https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html
	Refer from 
	Using conditionals in loops