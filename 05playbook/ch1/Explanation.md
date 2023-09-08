1. Hosts and Users
-------------------------------------------
---
- hosts: webservers
  remote_user: root
-------------------------------------------

	For each play define 
		define machines 
		remote user 
	to complete the steps (called tasks) as.

	hosts  
	------
		list of one or more groups or host patterns
		separated by colons
	More details: https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns


	remote_user 
	-----------
	create users on remote systems
	formerly called : "user".
	Renamed in Ansible 1.4 
		to more appropriate name

remote_user can be defined per task as follows
-------------------------------------------
---
- hosts: webservers
  remote_user: root
  tasks:
    - name: test connection
      ping:
      remote_user: yourname
-------------------------------------------
	  
	  
running tasks as another user 
-------------------------------------------
---
- hosts: webservers
  remote_user: yourname
  become: yes
-------------------------------------------
  
use keyword become on a particular task instead of the whole play
	i.e. execute one or more task as a particular user
-------------------------------------------
---
- hosts: webservers
  remote_user: yourname
  tasks:
    - service:
        name: nginx
        state: started
      become: yes
      become_method: sudo
-------------------------------------------

	  
login as another user, and then become a user different than root:
-------------------------------------------
---
- hosts: webservers
  remote_user: yourname
  become: yes
  become_user: postgres
-------------------------------------------

You can also use other privilege escalation methods, like su:
-------------------------------------------
---
- hosts: webservers
  remote_user: yourname
  become: yes
  become_method: su
-------------------------------------------
  
If you need to specify a password for sudo, run ansible-playbook with --ask-become-pass or -K. If you run a playbook utilizing become and the playbook seems to hang, it’s probably stuck at the privilege escalation prompt and can be stopped using Control-C, allowing you to re-execute the playbook adding the appropriate password.

Important

When using become_user to a user other than root, the module arguments are briefly written into a random tempfile in /tmp. These are deleted immediately after the command is executed. This only occurs when changing privileges from a user like ‘bob’ to ‘timmy’, not when going from ‘bob’ to ‘root’, or logging in directly as ‘bob’ or ‘root’. If it concerns you that this data is briefly readable (not writable), avoid transferring unencrypted passwords with become_user set. In other cases, /tmp is not used and this does not come into play. Ansible also takes care to not log password parameters.

New in version 2.4.

You can also control the order in which hosts are run. The default is to follow the order supplied by the inventory:
-------------------------------------------
- hosts: all
  order: sorted
  gather_facts: False
  tasks:
    - debug:
        var: inventory_hostname
-------------------------------------------


Possible values for order are:

inventory:
The default. The order is ‘as provided’ by the inventory
reverse_inventory:
As the name implies, this reverses the order ‘as provided’ by the inventory
sorted:
Hosts are alphabetically sorted by name
reverse_sorted:
Hosts are sorted by name in reverse alphabetical order
shuffle:
Hosts are randomly ordered each run
Tasks list
Each play contains a list of tasks. Tasks are executed in order, one at a time, against all machines matched by the host pattern, before moving on to the next task. It is important to understand that, within a play, all hosts are going to get the same task directives. It is the purpose of a play to map a selection of hosts to tasks.

When running the playbook, which runs top to bottom, hosts with failed tasks are taken out of the rotation for the entire playbook. If things fail, simply correct the playbook file and rerun.

The goal of each task is to execute a module, with very specific arguments. Variables can be used in arguments to modules.

Modules should be idempotent, that is, running a module multiple times in a sequence should have the same effect as running it just once. One way to achieve idempotency is to have a module check whether its desired final state has already been achieved, and if that state has been achieved, to exit without performing any actions. If all the modules a playbook uses are idempotent, then the playbook itself is likely to be idempotent, so re-running the playbook should be safe.

The command and shell modules will typically rerun the same command again, which is totally ok if the command is something like chmod or setsebool, etc. Though there is a creates flag available which can be used to make these modules also idempotent.

Every task should have a name, which is included in the output from running the playbook. This is human readable output, and so it is useful to provide good descriptions of each task step. If the name is not provided though, the string fed to ‘action’ will be used for output.

Tasks can be declared using the legacy action: module options format, but it is recommended that you use the more conventional module: options format. This recommended format is used throughout the documentation, but you may encounter the older format in some playbooks.

Here is what a basic task looks like. As with most modules, the service module takes key=value arguments:

-------------------------------------------
tasks:
  - name: make sure apache is running
    service:
      name: httpd
      state: started
-------------------------------------------

The command and shell modules are the only modules that just take a list of arguments and don’t use the key=value form. This makes them work as simply as you would expect:
-------------------------------------------
tasks:
  - name: enable selinux
    command: /sbin/setenforce 1
-------------------------------------------
	
The command and shell module care about return codes, so if you have a command whose successful exit code is not zero, you may wish to do this:
-------------------------------------------
tasks:
  - name: run this command and ignore the result
    shell: /usr/bin/somecommand || /bin/true
-------------------------------------------
	
Or this:
-------------------------------------------
tasks:
  - name: run this command and ignore the result
    shell: /usr/bin/somecommand
    ignore_errors: True
-------------------------------------------

If the action line is getting too long for comfort you can break it on a space and indent any continuation lines:

tasks:
  - name: Copy ansible inventory file to client
    copy: src=/etc/ansible/hosts dest=/etc/ansible/hosts
            owner=root group=root mode=0644
Variables can be used in action lines. Suppose you defined a variable called vhost in the vars section, you could do this:
-------------------------------------------
tasks:
  - name: create a virtual host file for {{ vhost }}
    template:
      src: somefile.j2
      dest: /etc/httpd/conf.d/{{ vhost }}
-------------------------------------------
	  
Those same variables are usable in templates, which we’ll get to later.

Now in a very basic playbook all the tasks will be listed directly in that play, though it will usually make more sense to break up tasks as described in Creating Reusable Playbooks.

Action Shorth