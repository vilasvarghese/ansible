1. Ping aws group
	ansible -i hosts -m ping aws

2. Ping aws host
	ansible -i hosts -m ping aws1

3. Ping combination of on prem and aws
	ansible -i hosts -m ping all