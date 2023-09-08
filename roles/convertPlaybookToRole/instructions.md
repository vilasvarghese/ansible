Reference: https://www.learnlinux.tv/getting-started-with-ansible-14-roles/

Replace all task entries with a role defintion as follows

---------------------------------------------------------------------------------------------

---
- hosts: all
  become: true
  pre_tasks:

  - name: update repository index (CentOS)
    tags: always
    yum:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "CentOS"

  - name: update repository index (Ubuntu)
    tags: always
    apt:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "Ubuntu"

#copy of the ssh not required
#- hosts: all
#  become: true
#  roles:
#    - base
   
- hosts: workstations
  become: true
  roles:
    - workstations

- hosts: web_servers
  become: true
  roles:
    - web_servers

- hosts: db_servers
  become: true
  roles:
    - db_servers

- hosts: file_servers
  become: true
  roles:
    - file_servers
---------------------------------------------------------------------------------------------	
	
	


mkdir roles

Create a directory for each role you wish to add:
	 cd roles
	 mkdir base
	 mkdir db_servers
	 mkdir file_servers
	 mkdir web_servers
	 mkdir workstations

Inside each role directory, create a tasks directory

	 mkdir base/tasks
		this we have already completed. copying the key.
		
	 mkdir db_servers/tasks
	 mkdir file_servers/tasks
	 mkdir web_servers/tasks
	 mkdir workstations/tasks

	cd tasks

Ignore the below section (this is copying ssh key which we have already completed)
--------------------------------------------------------------------------------- 
main.yml (base role)

Note: Use your actual key below on the last line, in place of the one you see here.

- name: add ssh key for simone
  authorized_key:
    user: simone
    key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAe7/ofWLNBq3+fRn3UmgAizdicLs9vcS4Oj8VSOD1S/ ansible"
--------------------------------------------------------------------------------- 

Set up required files/folders for db_servers role
 cd ..
 cd ..

 cd db_servers/tasks
 vi main.yml

main.yml (db_servers role)
---------------------------------------------------------------------------------------------
- name: install mariadb server package (CentOS)
  tags: centos,db,mariadb
  yum:
    name: mariadb
    state: latest
  when: ansible_distribution == "CentOS"

- name: install mariadb server
  tags: db,mariadb,ubuntu
  apt:
    name: mariadb-server
    state: latest
  when: ansible_distribution == "Ubuntu"
---------------------------------------------------------------------------------------------


 cd ../../file_servers/tasks
main.yml (file_servers role)
---------------------------------------------------------------------------------------------
- name: install samba package
  tags: samba
  package:
    name: samba
    state: latest
---------------------------------------------------------------------------------------------


cd ../../workstations/tasks
main.yml (workstations role)
---------------------------------------------------------------------------------------------
- name: install unzip
  package:
    name: unzip

- name: install terraform
  unarchive:
    src: https://releases.hashicorp.com/terraform/0.12.28/terraform_0.12.28_linux_amd64.zip
    dest: /usr/local/bin
    remote_src: yes
    mode: 0755
    owner: root
    group: root
---------------------------------------------------------------------------------------------


cd ../../web_servers/tasks
main.yml (web_servers role)
---------------------------------------------------------------------------------------------
- name: install httpd package (CentOS)
  tags: apache,centos,httpd
  yum:
    name:
      - httpd
      - php
    state: latest
  when: ansible_distribution == "CentOS"

- name: start and enable httpd (CentOS)
  tags: apache,centos,httpd
  service:
    name: httpd
    state: started
    enabled: yes
  when: ansible_distribution == "CentOS"

- name: install apache2 package (Ubuntu)
  tags: apache,apache2,ubuntu
  apt:
    name:
      - apache2
      - libapache2-mod-php
    state: latest
  when: ansible_distribution == "Ubuntu"

- name: change e-mail address for admin
  tags: apache,centos,httpd
  lineinfile:
    path: /etc/httpd/conf/httpd.conf
    regexp: '^ServerAdmin'
    line: ServerAdmin somebody@somewhere.net
  when: ansible_distribution == "CentOS"
  register: httpd

- name: restart httpd (CentOS)
  tags: apache,centos,httpd
  service:
    name: httpd
    state: restarted
  when: httpd.changed    

- name: copy html file for site
  tags: apache,apache,apache2,httpd
  copy:
    src: default_site.html
    dest: /var/www/html/index.html
    owner: root
    group: root
    mode: 0644
---------------------------------------------------------------------------------------------	 