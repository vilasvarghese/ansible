Major difference between 
	import and 
	include 
		tasks:

import tasks 
	will be parsed at the beginning 
		when you run your playbook
include tasks 
	will be parsed at the moment 
		Ansible hits them


Ansible support following import 
	import_tasks
	import_role
	
	include_vars
	include_role
	include_tasks
	
	



https://chewonice.com/2022/01/18/ansible-import-vs-include-loops/

In the following example there is no difference between include and import

import_a_task.yaml

---
- name: import a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: import a task
      import_tasks: subtask.yaml
	  
	  

include_a_task.yaml

---
- name: include a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: include a task
      include_tasks: subtask.yaml
	  
	  
subtask.yaml

---
- name: Debug some stuff
  debug:
    msg: "This is a subtask"



You’ll notice the output is similar but the include task contains a few extra lines that show which task was included and ran.


Difference between import and include become apparent when we look at using loops, tags, and conditionals.

import_b_task.yaml

---
- name: import a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: import a task
      import_tasks: subtask.yaml
      loop:
        - 123
        - 456
        - 789
		
include_b_task.yaml

---
- name: include a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: include a task
      include_tasks: subtask.yaml
      loop:
        - 123
        - 456
        - 789

subtask.yaml

---
- name: Debug some stuff
  debug:
    msg: "{{ item }}"

import_b_task.yaml fails to execute while include_b_task passes



Let's move the loop to the subtask


import_c_task.yaml

---
- name: import a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: import a task
      import_tasks: subtask.yaml

	
	
subtask.yaml

---
- name: debug 1
  debug:
    msg: "Loop 1: {{ item }}"
  loop:
    - 123
    - 456
    - 789

- name: debug 2
  debug:
    msg: "Loop 2: {{ item }}"
  loop:
    - 123
    - 456
    - 789


Include_tasks: define the loop one time in the outermost task.

include_c_task.yaml

---
- name: include a task
  hosts: localhost
  gather_facts: false
  tasks:
    - name: include a task
      include_tasks: subtask.yaml
      loop:
        - 123
        - 456
        - 789


subtask.yaml

---
- name: debug 1
  debug:
    msg: "Loop 1: {{ item }}"

- name: debug 2
  debug:
    msg: "Loop 2: {{ item }}"
	
	
Also N.B:
	import_tasks – 
		each subtask executes its entire loop before going to the next subtask. 
	include_tasks – 
		each item in the loop is run against all subtasks one-by-one before moving onto the next 
	
	Verify this in the output.
	
	