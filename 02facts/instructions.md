Refer 


1: https://www.redhat.com/sysadmin/playing-ansible-facts
2: https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html
3: https://www.middlewareinventory.com/blog/ansible-facts-list-how-to-use-ansible-facts/




Below instructions may not be required.



1. Create following directory on managed nodes.
								--------------
	/etc/ansible/facts.d 
	
2. Place facts file with extension .fact inside above directory.
	File can be written in 
		python
		shell
		perl 
		any other languages.
	Output of the fact should be json

3. Give execution permission on the file.

4. #sudo su
5. cd
6. mkdir facts.d
7. sudo yum install git -y
	sudo apt install git -y
8. yum install httpd -y
	sudo apt install apache2 -y 
9. cd facts.d
10. git --version | awk '{print $3}'
	will return just the version number
11. httpd -version | awk 'NR==1 { print $3}'
12. vi myversion.fact
------------------------------------------------
#!/bin/bash
git_version=$(git --version | awk '{print $3}')
httpd_version=$(httpd --version | awk 'NR==1 { print $3}')

cat << EOF
{
  "Git_Version": "$git_version",
  "httpd_version": "$httpd_version"
}
EOF
------------------------------------------------
13. chmod +x ./myversion.fact
	./myversion.fact
	Execute and verify that it generates a json response.
	
14. ansible localhost -m setup | egrep -i "(Git_Version|httpd)"
	should display httpd version and git version on the master.
	
	or 
	ansible localhost -m setup -a "filter=ansible_local"
	
15. Copy the file into all managed nodes
	a. ansible all -m file -a "path=/etc/ansible/facts.d state=directory" -b
	
16. cat /etc/ansible/facts.d/myversion.fact 
17. ansible all -m copy -a "src=/etc/ansible/facts.d/myversion.fact dest=/etc/ansible/facts.d/myversion.fact mode='0755' -b 

18. Collect the git and httpd version
	ansible all -m setup | egrep -i "(Git_Version|httpd)"