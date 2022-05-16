1. Install docker on client using installdocker.yml
	this should install docker
	alternatively install docker directly
	and start it
	give user vilas access to group docker
		useradd -aG docker vilas
	

2. On the client 
	Python plugin for docker is required.
		Refer InstallScript
			upgrade pip on client machine

		python3.10 -m pip install --upgrade pip
		pip3.10 install docker
		#pip3.10 install --upgrade docker

	OR 

		sudo su
		python3  -m pip install --upgrade pip
		pip3 install docker
		python3 location may be /usr/bin/python3


3. On server verify the ansible and python version
	docker should work with ansible 2.11.11 pthon 3.6.8
	
	python3.10 -m pip install ansible
	python3.10 -m pip install --upgrade ansible
	
	or remove and reinstall
