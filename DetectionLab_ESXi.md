# ESXI 6.7 (working)

## build
### install exsi
### install wsl
### build vm on esxi
## things

## Build Procedure
### ESXi installation(https://clo.ng/blog/detectionlab-on-esxi/):
- register account, get a license key, and download the ESXi .ISOs
	https://www.vmware.com/go/get-free-esxi
- create a bootable USB using Rufus (https://rufus.ie/)
- install ESXi using bootable USB

#### prereq:
	https://detectionlab.network/introduction/prerequisites/
	https://detectionlab.network/deployment/esxi/
- ESXi 6.x
- Vagrant 2.2.9+
- Packer v1.7.0+ and in PATH
- Terraform v0.13+
- OVFTool in 'PATH'
	https://code.vmware.com/web/tool/4.4.0/ovf
- Ansible and pywinrm in 'PATH'
	run: "pip3 install ansible pywinrm --user"
	(this might require step installing python & pip3)
- sshpass 
	run: "setup-x86_64.exe -q -s http://cygwin.mirror.constant.com -P sshpass" where 'setup-x86_64.exe' locate
	(this requires installing cygwin from https://cygwin.com/install.html)
	(installation step inspired from: https://stackoverflow.com/questions/37243087/how-to-install-sshpass-on-windows-through-cygwin)

- Git clone DetectionLab to local device:
	run: "git clone https://github.com/clong/DetectionLab.git"
	
- On ESXi host:
	Enable SSH
	Enable "Guest IP Hack"
	Open VNC ports on the firewall
		Openning VNC ports allows 'windows_10_esxi' and 'ubuntu2004_esxi' accessibility to ESXi host
	Instructions for above:
		https://nickcharlton.net/posts/using-packer-esxi-6.html
		to edit files on ESXi host, use 'vi' comment (https://kb.vmware.com/s/article/1020302)

#### Step 1 - Edit configuration files
- in 'DetectionLab/ESXi/Packer/variables.json'
	match variable to ESXi host environment
	usually set 'esxi_network_with_dhcp_and_internet' to "VM Network"
- for ESXi 6.x host
	remove 
		“vnc_over_websocket”: true,
		“insecure_connection”: true,
	from 'builders' array under 
		DetectionLab/ESXi/Packer/windows_10_esxi.json
		DetectionLab/ESXi/Packer/windows_2016_esxi.json
		DetectionLab/ESXi/Packer/ubuntu2004_esxi.json
#### Steps 2 - build vms
From DetectionLab/ESXi/Packer
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_10_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_2016_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json ubuntu2004_esxi.json
From DetectionLab/ESXi
	create terraform.tfvars to override - https://www.terraform.io/language/values/variables#variable-definitions-tfvars-files

	
#### draft notes
C:\ewalker\install pack
C:\ewalker\install pack\setup-x86_64.exe -q -s http://cygwin.mirror.constant.com -P sshpass 


{
    "esxi_host": "192.168.30.181",
    "esxi_datastore": "datastore1",
    "esxi_username": "root",
    "esxi_password": "eWalker123",
    "esxi_network_with_dhcp_and_internet": "VM Network"
}


cd d:\work\ewalker\DetectionLab\ESXi\Packer


### Set up WSL
	reference: https://docs.microsoft.com/en-us/windows/wsl/install-manual

#### Prerequisites
	For x64 systems: Version 1903 or later, with Build 18362 or later.
	For ARM64 systems: Version 2004 or later, with Build 19041 or later.

#### Step 1 - Enabling the WSL Windows Feature
- run PowerShell as Administrator
	run: dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart 
- Restart machine to complete the WSL install and update to WSL 2

#### Step 2 - Download the Linux kernel update package
- Download the latest package from: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
- install the update package
- Set WSL 2 as your default version
	run: wsl --set-default-version 2

#### Step 3 - Installing a Linux Distribution for WSL
	reference: https://docs.microsoft.com/en-us/windows/wsl/install
- to see a list of available distributions
	run: wsl --list --online
- to install a specific distribution
	run: wsl --install -d <DistroName>
- create a User Name and Password
	reference: https://docs.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password

#### Step 4 - Installing Windows Terminal
- install from: https://aka.ms/terminal
	reference: https://docs.microsoft.com/en-us/windows/terminal/install

#### Step 5 - Set up wsl
- install pip 
	run: sudo apt update && sudo apt upgrade
	run: sudo apt install python3-pip3

### DetectionLab Deployment
	reference: https://detectionlab.network/deployment/esxi/

#### install software prerequisites
- install Terraform v0.13+ (https://www.terraform.io/downloads),
	Packer 1.6.0+ (https://www.packer.io/downloads),
	Vagrant 2.2.9+ (https://www.vagrantup.com/downloads)
	run:
		curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
		sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
		sudo apt-get update && sudo apt-get install terraform
		sudo apt-get update && sudo apt-get install packer
		sudo apt-get update && sudo apt-get install vagrant
- install OVFTool
	download OVFTool from: https://developer.vmware.com/web/tool/4.4.0/ovf
	
	sudo apt-get install zip unzip
	
	echo 'PATH=$HOME/ovftool:$PATH' >> ~/.bashrc
- install pywinrm and ansible and add ansible to PATH
	reference?: https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#windows-faq-ansible
	pip3 install pywinrm --user
	pip3 install ansible --user
	echo 'PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
	source .bashrc
- git clone DetectionLab: 
	mkdir ~/code && cd ~/code
	git clone https://github.com/clong/DetectionLab.git
- install sshpass
	sudo apt install sshpass

#### set up vms
PACKER_LOG=1 packer build -var-file variables.json ubuntu2004_esxi.json

### Set up WSL 1

## Things to Notice
