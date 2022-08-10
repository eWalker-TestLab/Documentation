# ESXI 7.0U3 (ubuntu in VMware) (NOT DONE)
## Build Procedure

### Step 1 - Prepare ESXi: [ref.](https://www.virten.net/2021/11/vmware-esxi-7-0-update-3-on-intel-nuc/)///
- download ESXi 7.0 offline bundle from [here](https://customerconnect.vmware.com/en/web/vmware/evalcenter?p=free-esxi7)
- download Network Community Driver from [here](https://flings.vmware.com/community-networking-driver-for-esxi)
- run commands as [here](http)
- install ESXi using bootable USB using [Rufus](http)
- reference to the guide from [here](https://clo.ng/blog/detectionlab-on-esxi/) and [here](https://nickcharlton.net/posts/using-packer-esxi-6.html) to set up ESXi host networking and configuration

### Step 2 - Prepare Ubuntu:
- run all commands as root user or with sudo
- change <some path> to your corresponding path
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
4. install ansible and pywinrm, and add ansible to $PATH
	```
	pip3 ansible pywinrm
	echo 'export PATH="<path to ansible>:$PATH"' >> ~/.bashrc
	source ~/.bashrc
	```
5. git clone DetectionLab to local device:
	`git clone https://github.com/clong/DetectionLab.git`

### Step 3 - Edit configuration files
- reference to the [step](https://detectionlab.network/deployment/esxi/#steps) to deploy the DetectionLab

### Step 4 - prepare VMs:
- From "<some path>/DetectionLab/ESXi/Packer", run:
	```
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_10_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json windows_2016_esxi.json
	PACKER_CACHE_DIR=../../Packer/packer_cache packer build -var-file variables.json ubuntu2004_esxi.json
	```

### Step 5 - terraform ///
- locate to "DetectionLab/ESXi", create and edit a "terraform.tfvars" file
	```
	esxi_hostname = "192.168.1.224"
	esxi_password = ""
	esxi_datastore = "datastore1"
	vm_network = "VM Network"
	hostonly_network = "HostOnly" 
	```
- creagte logger, dc, wef, win10
	run 
	```
	terraform init && terraform apply
	```

### Step 6 - ansible
- locate to "DetectionLab/ESXi/ansible" and edit "inventory.yml" as [step8. here](https://detectionlab.network/deployment/esxi/)
- take snapshot on VMs before ansible playlook
- run
	``` 
	ansible-playbook -v detectionlab.yml
	```


## Things to Notice
\1. ensure the ansible config file is from "DetectionLab/ESXi/ansible/ansible.cfg"
```
cd ~/code/DetectionLab/ESXi/ansible
ansible --version
```
\2. you can use following syntax to send out debug log:
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
\3. you can use following syntax to send out debug log
```
ansible-playbook -v detectionlab.yml -t "[logger/dc/wef/win10]" &> ~/code/log/ansible_playbook/[logger/dc/wef/win10]_1.log
tail -f ~/code/log/ansible_playbook/[logger/dc/wef/win10]_1.log
```

\4. When setting up the VMs, it is advice to work under a isolated network. (i.e. only minimum devices like ESXi host and current machine conneted under a router)

\5. When two ESXi host running same VMs set up script, network on both host would receive same address. When an admin try to work on the lab environment inside VMs, it is uncertain which ESXi host does does the VM locate. \
Sol. 
- change the mac address inside "DetectionLab/ESXi/main.tf".
- change the mac address respectively in "ESXi/ansible/roles/dc/tasks/main.yml", "ESXi/ansible/roles/wef/tasks/main.yml, and "ESXi/ansible/roles/win10/tasks/main.yml"

## Reference 
guide esxi install: https://clo.ng/blog/detectionlab-on-esxi/

guide DetectionLab deployment: https://detectionlab.network/deployment/esxi/

guide esxi host config: https://nickcharlton.net/posts/using-packer-esxi-6.html

[id 1]: https://www.virten.net/2021/11/vmware-esxi-7-0-update-3-on-intel-nuc/