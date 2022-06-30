# ESXI 6.7 (all build with ubuntu in VMware) (NOT DONE)
## Build Procedure
### Step 1 - Prepare ESXi:
- register account, get a license key, and download the ESXi .ISOs from: https://www.vmware.com/go/get-free-esxi
- create a bootable USB using [Rufus](https://rufus.ie/)
- install ESXi using the bootable USB
- reference to the guide from [here](https://clo.ng/blog/detectionlab-on-esxi/) and [here](https://nickcharlton.net/posts/using-packer-esxi-6.html) to set up ESXi host networking and configuration

### Step 2 - Prepare Ubuntu:
- run all commands as root user or with sudo
- have your packages up-to-date
- reference to: https://detectionlab.network/deployment/esxi/
1. ensure curl, git, pip3, sshpass installed
	```
	sudo apt install curl git python3-pip
	```
2. install [Terraform](https://www.terraform.io/downloads) and [Packer](https://www.packer.io/downloads)
	```	
	curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
	sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
	sudo apt-get update && sudo apt-get install terraform && sudo apt-get install packer
	```
3. install [OVFTool](https://developer.vmware.com/web/tool/4.4.0/ovf) and add it to $PATH
	```
	echo 'export PATH="<path to ovftool file>:$PATH"' >> ~/.bashrc
	source ~/.bashrc
	```
4. install ansible and pywinrm, and and add ansible to $PATH
	```
	pip3 ansible pywinrm
	echo 'export PATH="<path to ansible>:$PATH"' >> ~/.bashrc
	source ~/.bashrc
	```
5. git clone DetectionLab to local device:
	`git clone https://github.com/clong/DetectionLab.git`

### Step 3 - Edit configuration files
- reference to the [step](https://detectionlab.network/deployment/esxi/#steps) to deploy the DetectionLab

### Step 4 - Set up VMs:
- From "<some dir>/DetectionLab/ESXi/Packer", run:
	```
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_10_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_2016_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json ubuntu2004_esxi.json
	```

### Step 5 - Create VMs {{{WORKING}}}


## Things to Notice
1.ensure the ansible config file is from "DetectionLab/ESXi/ansible/ansible.cfg"
```
cd ~/code/DetectionLab/ESXi/ansible
ansible --version
```
2.you can use following syntax to send out debug log:
```
PACKER_CACHE_DIR=../../Packer/packer_cache PACKER_LOG=1 packer build -var-file variables.json windows_10_esxi.json &> ~/code/log/packer_build_windows_10_esxi_1.log
PACKER_CACHE_DIR=../../Packer/packer_cache PACKER_LOG=1 packer build -var-file variables.json windows_2016_esxi.json &> ~/code/log/packer_build_windows_2016_esxi_1.log
PACKER_CACHE_DIR=../../Packer/packer_cache PACKER_LOG=1 packer build -var-file variables.json ubuntu2004_esxi.json &> ~/code/log/packer_build_ubuntu2004_esxi_1.log
```
In another terminal: 
```
tail -f ~/code/log/packer_build_windows_10_esxi_1.log
tail -f ~/code/log/packer_build_windows_2016_esxi_1.log
tail -f ~/code/log/packer_build_ubuntu2004_esxi_1.log
```


## Reference 
guide esxi install: https://clo.ng/blog/detectionlab-on-esxi/

guide DetectionLab deployment: https://detectionlab.network/deployment/esxi/

guide esxi host config: https://nickcharlton.net/posts/using-packer-esxi-6.html

