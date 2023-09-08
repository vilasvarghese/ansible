Reference: https://goetzrieger.github.io/ansible-collections/2-using-collections-from-playbooks/

Ansible Collections 
	new distribution format for Ansible 
		like roles
	can include 
		playbooks
		roles, 
		modules
		plugins. 
	
	Modules are moved from the core Ansible repository 
		into collections living in repositories outside of the core repository. 
	
Before collections, 
	module creators had to wait for their modules to be included in an upcoming Ansible release 
	or had to add them to roles, 
	which made consumption and management more difficult.
By distributing modules packaged in Ansible Content Collections along with 
	roles
	documentation and 
	playbooks, 
		content creators are now able to move as fast or conservative as the technology they manage demands.
Example: A public cloud provider could make new functionality of an existing service available, 
	that could be rolled out along with the ability to automate the new functionality with Ansible. 
	With Ansible Collections the author doesn’t have to wait for the next Ansible release and can instead roll out the new content independently. 
	Prior to Ansible Collections the author had to wait for the next Ansible release.


Ansible Collections are 
	format in which Ansible content like 
		Modules, 
		Roles, 
		Playbooks and 
		Plugin, etc. 
			can be distributed to the Ansible users 
			
	This distribution is performed by Ansible Galaxy
	Ansible Galaxy, 
		place from where you can install or get other developer’s shared content. 
		We can upload our code/content in Ansible Galaxy 
	There is a global location for the Ansible Galaxy server 
		https://galaxy.ansible.com/
		other Galaxy Server possible
			like RedHat Automation Hub, 
		can configure the same in your Ansible setup.




For Ansible users, 
	updated content is continuously available. 
	Managing content is easier as 
		modules, 
		plugins, 
		roles, and 
		docs 
			that belong together are packaged together and versioned.
			
			
Fully Qualified Collection Name
--------------------------------
Ansible Collection names 
	combination of two components. 
		First part: 
			author's name 
				wrote and maintains Ansible Collection. 
		Second part 
			Ansible Collection's name. 
		
		An author can multiple collections
		Ansible Collections can have multiple authors

<author>.<collection>
---------------------
These are examples for Ansible Collection names:
	ansible.posix
	geerlingguy.k8s
	theforeman.foreman
	
Search for ansible.posix in https://galaxy.ansible.com/
	search for 
		ansible 
		posix
		geerlingguy
	

To identify a specific module in an Ansible Collection, 
	we add the name of it as the third part:
	
	<author>.<collection>.<module>

Examples for a fully qualified Ansible Collection Name:
	ansible.posix.selinux
	geerlingguy.k8s.kubernetes
	theforeman.foreman.user			
	
	
	
Explaining Ansible Collections
Ansible Collections 
	collected from Ansible Galaxy 
		similar to import any roles. 
	Syntantical changes 
	
Points which you must remember while working on Collections.

To install a collection
	use -p to specify the location of Ansible collection
	this is set by COLLECTIONS_PATHS in Ansible configuration file and by default ~/.ansible/collections:/usr/share/ansible/collections.
ansible-galaxy collection install<collection_name> -p <collection_local_location>
	can place in
		collections/ansible_collections/ adjacent to your playbook. 
		This will follow the below directory structure.

my_playbook.yaml
├── collections/
│   └── ansible_collections/
│               └── our_Ansible_namespace/
│                   └── our_Ansible_collection/< a playbook relevant collection's structure>


default Galaxy server 
	https://galaxy.ansible.com, 
to change 
	1. using ansible.cfg file
		modify your working cfg file
			use a section named [galaxy]
				multiple Galaxy server can be configured.
				place a list of collections after server_list
	2. using command line
		–server in the command 
			limit your collection’s search. 

Please refer to the below section
------------------------------------------------------------------
[galaxy] server_list = automation_hub, sample_galaxy_collection
[galaxy_server.automation_hub] url=https://cloud.redhat.com/api/automation-hub/
auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
token=automation_hub_token
[galaxy_server.sample_galaxy_collection] url=https://galaxy-dev.ansible.com/
token=sample_token

------------------------------------------------------------------

Below is the list for same: –

url: 
	Location of an Ansible Collection.
token: 
	mutually exclusive with the username. 
	used to authenticate against a Galaxy Server.
auth_url: 
	mutually exclusive with the username. 
	auth_url requires a token. 
	So this provides the location of the token endpoint

username: 
	This is mutually exclusive with token 
	used for basic authentication with Galaxy Server.
password: 
	password for the username above.

When installing any collection from Ansible Galaxy, 
	default latest version will be installed. 
	To install specific version 
:* For any version, this is default and will look for the latest.
:!= For any version by not equal to specified ones.
:== For a specified version.
:>= For any version greater than equal to the one specified here.
:> For any version greater than specified.
:<= For any version lower than equal to the one specified here.
:< For any version lower than specified.

Instead of installing on the command line, 
	you can set up in yaml, 
	give the list of collections that need to be installed under keyword collections. 
	This can also work when you have defined your roles in the same file. 

A sample file will be like below: –
------------------------------------------------------------------------
---
collections:
# Installing collection from Ansible_Galaxy.
- name: <namespace_name.collection_name>
source: https://galaxy.ansible.com
roles:
# Installing role from Ansible_Galaxy.
- name: <role_name>
------------------------------------------------------------------------

use the below command to install Collections.
	ansible-galaxy collection install -r requirements.yml -p ./<file_location>




After installation, 
	to use an Ansible collection inside a playbook, 
		refer it by using it FQCN (fully qualified collection name). 
	This kind of reference works for 
		modules, 
		plugins 
		roles. 

Below is a code sample.
------------------------------------------------------------------------
---
- hosts: all
collections:
- customized_namespace.customized_collection
tasks:
- import_role:
name: <role_name>
- my_module:
key: value
------------------------------------------------------------------------

	
	
	
	
	
	
	
	
	
	
	
	
	
Collections Lookup
------------------
Ansible Collections 
	use a simple method to define collection namespaces. 
	If your playbook loads collections using the collections key and one or more roles, 
	then the roles will not inherit the collections set by the playbook.

This leads to the main topic of this exercise: 
	roles have an independent collection loading method 
		based on the role’s metadata. 
To control collections search for the tasks inside the role, 
	users can choose between two approaches:

Approach 1: Pass a list of collections in the collections field inside the meta/main.yml file within the role. This will ensure that the collections list searched by the role will have higher priority than the collections list in the playbook. Ansible will use the collections list defined inside the role even if the playbook that calls the role defines different collections in a separate collections keyword entry.


ansible-galaxy collection list | grep posix
ansible-galaxy collection list


# myrole/meta/main.yml
collections:
  - my_namespace.first_collection
  - my_namespace.second_collection
  - other_namespace.other_collection
Approach 2: Use the collection fully qualified collection name (FQCN) directly from a task in the role. In this way the collection will always be called with its unique FQCN, and override any other lookup in the playbook

- name: Create an EC2 instance using collection by FQCN
  amazon.aws.ec2:
    key_name: mykey
    instance_type: t2.micro
    image: ami-123456
    wait: yes
    group: webserver
    count: 3
    vpc_subnet_id: subnet-29e63245
    assign_public_ip: yes
Roles defined within a collection always implicitly search their own collection first, so there is no need to use the collections keyword in the role metadata to access modules, plugins, or other roles.

In the following chapters of this lab you will learn how collections work and see different examples to see how it works.


----------------------------
ansible-galaxy collection list | grep posix
ansible-galaxy collection list ansible.posix
ansible-galaxy collection list amazon.aws
ansible-galaxy collection list | grep newswangerd

ansible-galaxy collection install newswangerd.collection_demo
ansible-galaxy collection install newswangerd.collection_demo --ignore-certs



to use a module in collection_demo
ansible localhost -m newswangerd.collection_demo.real_facts


Module's by default get's installed to
		/home/<user>/.ansible/collections/ansible_collections
	
See the location's configured for installing ansible.	
		ansible-config dump | grep -i collection
		
		
To see the documentation
	ansible-doc ansible.posix.selinux
	#N.B: documentatio is not mandatory and there can be collections with doc.
	# in such cases you may get an error.
		
		
		
	There is no remove/delete/uninstall
	-----------------------------------
		ansible-galaxy has no method
		
	To remove (there is no command)
		cd /home/<user>/.ansible/collections/ansible_collections
			rm -rf newswangerd.collection_demo
			
ansible-galaxy collection install --help			
ansible-galaxy collection install -f ansible.posix


Using collection in a playbook.
------------------------------
module's in collection can be used directly.

SELinux 
	gives that extra layer of security to the resources in the system. 
	It provides the MAC (mandatory access control) as 
		contrary to the DAC (Discretionary access control). 
	SELinux can operate in any of the 3 modes :

1. Enforced : Actions contrary to the policy are blocked and a corresponding event is logged in the audit log.
2. Permissive : Actions contrary to the policy are only logged in the audit log.
3. Disabled : The SELinux is disabled entirely.

To enforce selinux.

ansible-playbook enforce-selenix.yml -K


ansible-galaxy collection --help


User ansible 2.9 and above
	ansible-galaxy collection install 
		uses https://galaxy.ansible.com as the Galaxy server

To install a collection hosted in Galaxy
	ansible-galaxy collection install <namespace>.<collection-name>

ansible-galaxy collection list | wc -l
ansible-galaxy collection install geerlingguy.k8s
ansible-galaxy collection list | wc -l

ansible-galaxy collection list

ansible-galaxy collection init vilascol
	ERROR! Invalid collection name 'vilascol', name must be in the format <namespace>.<collection>.
	Please make sure namespace and collection name contains characters from [a-zA-Z0-9_] only.



upgrade a collection 
	ansible-galaxy collection install my_namespace.my_collection --upgrade
	
	
	
	
Directly use the tarball from your build:
	ansible-galaxy collection install my_namespace-my_collection-1.0.0.tar.gz -p ./collections	
	
	-p: collection get's installed in ./collections
	

	ansible-galaxy collection install /path/to/collection -p ./collections


	or offline installation
	ansible-galaxy collection download <collection name>
	ansible-galaxy collection install <downloaded.tarball> -p ./collections	
	

To install multiple collections in a namespace directory.

ns/
├── collection1/
│   ├── MANIFEST.json
│   └── plugins/
└── collection2/
    ├── galaxy.yml
    └── plugins/
	
ansible-galaxy collection install /path/to/ns -p ./collections


--------------------------------------------------------------------------------------

For more options refer 
	https://docs.ansible.com/ansible/latest/user_guide/collections_using.html


--------------------------------------------------------------------------------------



Running collections from an Ansible role
-----------------------------------------

	
	two options to use Ansible Collections in role. 
		1. specify the collection in roles metadata.
		2. use collection FQCN to call the related modules and plugins
		
	1. Using roles metadata
	-----------------------
	To keep it clean
	mkdir ansicol1
	cd ansicol1
	
	a. Create a simple role. 
		ansible-galaxy init 
		ansible-galaxy init --init-path roles selinux_manage_meta
		
	b. Edit a couple of files. 
		First edit the role metadata in roles/selinux_manage_meta/meta/main.yml 
			append the following lines at the end of the file
			[don’t change anything else]:
-----------------------------			
# Collections list
collections:
  - ansible.posix			
-----------------------------

	c. Edit tasks
		Edit the roles/selinux_manage_meta/tasks/main.yml file 
		Add the following tasks 
			(make sure to keep the YAML start --- in place):
-----------------------------
---
# tasks file for selinux_manage_meta
- name: Enable SELinux enforcing mode
  selinux:
    policy: targeted
    state: "{{ selinux_mode }}"

- name: Enable booleans
  seboolean:
    name: "{{ item }}"
    state: true
    persistent: true
  loop: "{{ sebooleans_enable }}"

- name: Disable booleans
  seboolean:
    name: "{{ item }}"
    state: false
    persistent: true
  loop: "{{ sebooleans_disable }}"
-----------------------------

	d. Defining the default values to the variables
		edit the roles/selinux_manage_meta/defaults/main.yml 
			to define default values for role variables:

-----------------------------
---
# defaults file for selinux_manage_meta
selinux_mode: enforcing
sebooleans_enable: []
sebooleans_disable: []	
-----------------------------


	e. Clean up the unused folders of the role:
	rm -rf roles/selinux_manage_meta/{tests,vars,handlers,files,templates}
	
tree
.
└── roles
    └── selinux_manage_meta
        ├── defaults
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        └── tasks
            └── main.yml


	f. create a simple playbook.yml file in the root folder
-----------------------------	
---
- hosts: localhost
  become: true
  vars:
    sebooleans_enable:
      - httpd_can_network_connect
      - httpd_mod_auth_pam
    sebooleans_disable:
      - httpd_enable_cgi
  roles:
    - selinux_manage_meta
-----------------------------


	g. Run the playbook and check the results:
		ansible-playbook playbook.yml




		
	2. Using collection FQCN : calling collection in roles.
	---------------------------------------------------------------------
	Let's edit the first role.
		
	a. 
	cd ..
	cp -R ansicol1 ansicol2
	
	b. Edit roles/selinux_manage_fqcn/tasks/main.yml to use FQCN for modules. 

---------------------------------------------------------------------
---
# tasks file for selinux_manage_fqcn
- name: Enable SELinux enforcing mode
  ansible.posix.selinux:
    policy: targeted
    state: "{{ selinux_mode }}"

- name: Enable booleans
  ansible.posix.seboolean:
    name: "{{ item }}"
    state: true
    persistent: true
  loop: "{{ sebooleans_enable }}"

- name: Disable booleans
  ansible.posix.seboolean:
    name: "{{ item }}"
    state: false
    persistent: true
  loop: "{{ sebooleans_disable }}"	
---------------------------------------------------------------------	

	c. Modify the playbook

-----------------------------	
---
- hosts: localhost
  become: true
  vars:
    sebooleans_enable:
      - httpd_can_network_connect
      - httpd_mod_auth_pam
    sebooleans_disable:
      - httpd_enable_cgi
  roles:
    - selinux_manage_fqcn
-----------------------------
	d. execute the playbook
		ansible-playbook playbook.yml
		
		
	

Creating a collection
---------------------


	
Ansible Collections have two default lookup paths that are searched.	
User scoped path 
	/home/<username>/.ansible/collections
System scoped path 
	/usr/share/ansible/collections


Standard directory and file structure of Ansible collections

collection/
├── docs/
├── galaxy.yml
├── plugins/
│   ├── modules/
│   │   └── module1.py
│   ├── inventory/
│   └── .../
├── README.md
├── roles/
│   ├── role1/
│   ├── role2/
│   └── .../
├── playbooks/
└── tests/


The plugins folder 
	holds plugins, 
	modules
	module_utils 
		can be reused in 
			playbooks and 
			roles.

The roles folder 
	hosts custom roles
	while all collection playbooks must be stored in the playbooks folder.

The docs folder 
	used for the collections documentation
	also main README.md file 
		used to describe the collection.

The tests folder  
	tests written for the collection.

The galaxy.yml file 
	contains all the metadata used in the Ansible Galaxy hub to index the collection. 
	Also list collection dependencies if any.


	a. Let's create a new directory
	mkdir ansicol3
	cd ansicol3

	b. ansible-galaxy collection init --init-path ansible_collections vilas.demo

	--init-path 
		define a custom path in which the skeleton will be initialized
	collection_name 
		should be always author.name
		
 tree
.
└── ansible_collections
    └── vilas
        └── demo
            ├── docs
            ├── galaxy.yml
            ├── plugins
            │   └── README.md
            ├── README.md
            └── roles		



	c. cd plugins/
		mkdir modules
		cd modules
vi demo_hello.py
----------------------------------------------------------------------------
#!/usr/bin/python

ANSIBLE_METADATA = {
    'metadata_version': '1.0',
    'status': ['preview'],
    'supported_by': 'vilas'
}

DOCUMENTATION = '''
---
module: demo_hello
short_description: A module that says hello in many languages
version_added: "2.8"
description:
  - "A module that says hello in many languages."
options:
    name:
        description:
          - Name of the person to salute. If no value is provided the default
            value will be used.
        required: false
        type: str
        default: Mariam
author:
    - Vilas Varghese (@vilas)
'''

EXAMPLES = '''
# Pass in a custom name
- name: Say hello to Vilas
  demo_hello:
    name: "Vilas Varghese"
'''

RETURN = '''
fact:
  description: Hello string
  type: str
  sample: Hello my friend name!
'''

import random
from ansible.module_utils.basic import AnsibleModule


FACTS = [
    "Hello {name}!",
    "Bonjour {name}!",
    "Hola {name}!",
    "Ciao {name}!",
    "Hallo {name}!",
    "Hei {name}!",
	"Namaste {name}!",
	"Namaskaram {name}!",
	"Namaskara {name}!",
]


def run_module():
    module_args = dict(
        name=dict(type='str', default='Mariam'),
    )

    module = AnsibleModule(
        argument_spec=module_args,
        supports_check_mode=True
    )

    result = dict(
        changed=False,
        fact=''
    )

    result['fact'] = random.choice(FACTS).format(
        name=module.params['name']
    )

    if module.check_mode:
        return result

    module.exit_json(**result)


def main():
    run_module()


if __name__ == '__main__':
    main()

----------------------------------------------------------------------------

	e. cd back to vilas/demo folder
	ansible-galaxy init --init-path roles vilasrole

	in vilas/demo folder
	vi roles/vilasrole/tasks/main.yml
----------------------------------------------------------------------------

---
# tasks file for vilasrole

- name: Generate greeting and store result
  demo_hello:
    name: "{{ friend_name }}"
  register: demo_greeting

- name: store test in /etc/vilas
  copy:
    content: "{{ demo_greeting.fact }}\n"
    dest: /etc/vilas
  become: yes
----------------------------------------------------------------------------


	f. vi vilasrole/defaults/main.yml
----------------------------------------------------------------------------
---
# defaults file for vilasrole
friend_name: "The right guy"
----------------------------------------------------------------------------

	g. cd vilasrole
		rm -rf handlers vars tests

	vi vilasrole/meta/main.yml

----------------------------------------------------------------------------
		
galaxy_info:
  author: Your name
  description: Vilas demo

  license: GPL-2.0-or-later

  min_ansible_version: 2.9

  galaxy_tags: ["demo"]

dependencies: []

----------------------------------------------------------------------------
	h.
	cd to vilas/demo folder
	ansible-galaxy collection build
	
	vilas-demo-1.0.0.tar.gz is created
	ansible-galaxy collection install vilas-demo-1.0.0.tar.gz



Test the collection created
---------------------------
	a. move back to home directory
		mkdir testcol
		cd testcol
		vi playbook.yml
----------------------------------------------------------------------------
---
- hosts: localhost

  tasks:
  - import_role:
      name: vilas.demo.vilasrole
    vars:
      friend_name: "Happy Vilas"
----------------------------------------------------------------------------

	ansible-playbook playbook.yml -K
	
	cat /etc/vilas












[deprecated section]

For the below refer the ansible documentation https://docs.ansible.com/ansible/latest/user_guide/collections_using.html

Using requirements.yml

	can set up a requirements.yml file 
		install multiple collections in one command. 
		This file is a YAML file in the format:

---
collections:
# With just the collection name
- my_namespace.my_collection

# With the collection name, version, and source options
- name: my_namespace.my_other_collection
  version: 'version range identifiers (default: ``*``)'
  source: 'The Galaxy URL to pull the collection from 
	(default: ``--api-server`` from cmdline)'


You can specify four keys for each collection entry:
	name
	version
	source
	type

The version key uses the same range identifier format documented in Installing an older version of a collection.
type can be 
	file, 
	galaxy, 
	git, 
	url, 
	dir, or 
	subdirs. 

If type is omitted, 
	name key is used to determine the source of the collection.

type: git, 
	version can be a branch or 
	git commit-ish object (commit or tag). 

For example:
collections:
  - name: https://github.com/organization/repo_name.git
    type: git
    version: devel









For more options to install refer 
	https://docs.ansible.com/ansible/latest/user_guide/collections_using.html




