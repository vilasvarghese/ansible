Pre-requisite

On Control node
	1. Ansible version 2.9 or above
	2. Python3 installed 
		yum install python3
		
	3.	pywinrm installed
		pip3 install "pywinrm>=0.3.0"
		or
		python3 -m pip install --user "pywinrm>=0.3.0"
		
On Windows		
	1. PowerShell 3.0 or latter
	2. .NET 4.0 to be installed
	3. WinRM listener should be created and activated.
	
Ensure to have latest version of ansible with latest version of ansible

Follow the instructions from the site



Define inventory
[win]
<ip>

[win:vars]
ansible_user=vilas
ansible_password=mypwd
ansible_connection=winrm
ansible_winrm_server_cert_validation=ignore


ansible win -m win_ping 

---
- hosts: win
  gather_facts: no
  tasks
  - name: Install Chocolatey on Windows10
    win_chocolatey: name=procexp state=present
	
	
	Follow below url for winrm setup on windows.
		https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html

	Follow below url for ansible connectivity
	https://www.ansiblepilot.com/articles/configure-a-windows-host-for-ansible-ansible-winrm/
	

	https://digitalistnetwork.com/talks/winrmansible/
	
On windows node	
	
	
1. Check powershell version
		$PSVersionTable	#$ is a part of the syntax - don't skip that.
		or Get-Host | Select-Object Version
		
		Verify the PSVersion is above 3.
		
	Verify .NET version
		Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse | Get-ItemProperty -Name version -EA 0 | Where { $_.PSChildName -Match '^(?!S)\p{L}'} | Select PSChildName, version
		
		
		
2. 
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/jborean93/ansible-windows/master/scripts/Upgrade-PowerShell.ps1"
$file = "$env:temp\Upgrade-PowerShell.ps1"
$username = "Administrator"
$password = "windows password"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force

# Version can be 3.0, 4.0 or 5.1
&$file -Version 5.1 -Username $username -Password $password -Verbose


3. 
# This isn't needed but is a good security practice to complete
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force

$reg_winlogon_path = "HKLM:\Software\Microsoft\Windows NT\CurrentVersion\Winlogon"
Set-ItemProperty -Path $reg_winlogon_path -Name AutoAdminLogon -Value 0
Remove-ItemProperty -Path $reg_winlogon_path -Name Administrator -ErrorAction SilentlyContinue
Remove-ItemProperty -Path $reg_winlogon_path -Name "windows password" -ErrorAction SilentlyContinue


4. 
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/jborean93/ansible-windows/master/scripts/Install-WMF3Hotfix.ps1"
$file = "$env:temp\Install-WMF3Hotfix.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file -Verbose

5. 
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file


6. winrm enumerate winrm/config/Listener


7. 

Ensure to replace the ceritificate thumbprint from the response of winrm enumerate winrm/config/Listener.

$selector_set = @{
    Address = "*"
    Transport = "HTTPS"
}
$value_set = @{
    CertificateThumbprint = "240D4CE5A43BC4F3D1FE885837F7F0335E089506"
}

New-WSManInstance -ResourceURI "winrm/config/Listener" -SelectorSet $selector_set -ValueSet $value_set


To get an output of the current service configuration options, run the following command:

8. Verify 
winrm get winrm/config/Service
winrm get winrm/config/Winrs

9. ############## To remove
# Remove all listeners
Remove-Item -Path WSMan:\localhost\Listener\* -Recurse -Force

# Only remove listeners that are run over HTTPS
Get-ChildItem -Path WSMan:\localhost\Listener | Where-Object { $_.Keys -contains "Transport=HTTPS" } | Remove-Item -Recurse -Force




On the master machine

	pip3 install pywinrm

	
	python3.10 -m pip install --upgrade pip
	python3.10 -m pip install pywinrm
	python3.10 -m pip install ansible
	
	python3 -m pip install --upgrade pip
	python3 -m pip install pywinrm
	python3 -m pip install ansible


Setup 


inventory 

[win]
<private ip> 
172.16.2.6 

[win:vars]
ansible_user=Administrator
ansible_password=<my pwd>
ansible_port=5986
ansible_connection=winrm
ansible_winrm_transport=basic
ansible_winrm_server_cert_validation=ignore




Verify 

ansible win -m win_ping

