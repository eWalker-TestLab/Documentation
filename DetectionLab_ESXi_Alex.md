# ESXi 6.7 on NUC8i7BEH (with Ubuntu 20.04 in VMware Workstation Pro 16)

## Build Procedure

Although this setup is done using Ubuntu 20.04 guest OS in VMware, the same should apply to native OS running bare metal.

### Prerequisites

#### Ubuntu Environment Configurations

1. Before starting building the lab, updating the system is recommended. This can be done by executing `sudo apt update` and then `sudo apt full-upgrade -y`. Finally, `reboot` the machine.

2. Install necessary tools by executing the following command:

   ```bash
   sudo apt install build-essential curl git gnupg software-properties-common
   ```

3. Install *Terraform* by executing the following commands (more info on [the official website](https://www.terraform.io/downloads)):

   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install terraform
   ```

4. Install *Vagrant* by executing the following commands (more info on [the official website](https://www.vagrantup.com/downloads)):

   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install vagrant
   ```

5. Install *Packer* by executing the following commands (more info on [the official website](https://www.packer.io/downloads)):

   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install packer
   ```

6. Install *Ansible* using the following commands. More details can be found in the [official documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

   ```bash
   sudo apt update
   sudo apt install software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt update
   sudo apt install ansible
   ```

7. Install `pywinrm` using the following command.

   ```bash
   pip3 install pywinrm
   ```

8. Install `sshpass` to allow *Ansible* to use password login by the following command:

   ```bash
   sudo apt install sshpass
   ```

9. To avoid a bug with *Ansible*, set an environment variable using the following command.

   ```bash
   echo 'export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES' >> ~/.bash_profile
   ```

10. Install `ovftool`. In Ubuntu, open a web browser and download [ovftool](https://developer.vmware.com/web/tool/4.4.0/ovf). Unzip the downloaded file and add its path to your `PATH` environment variable by the following commands. Refer [here](https://docs.vmware.com/en/VMware-Telco-Cloud-Operations/1.4.0/deployment-guide-140/GUID-95301A42-F6F6-4BA9-B3A0-A86A268754B6.html).

    ```bash
    unzip <YOUR DOWNLOADED FILE>
    echo 'export PATH="$PATH:<PATH TO YOUR UNZIPPED FOLDER>"' >> ~/.bash_profile
    ```

#### ESXi Environment Configurations

Follow the instructions [here](https://clo.ng/blog/detectionlab-on-esxi/) in the Software section and also [here](https://nickcharlton.net/posts/using-packer-esxi-6.html). These steps serve the following purposes.

1. The ESXi instance must have at least two separate networks - one that is accessible from your current machine and has internet connectivity and a HostOnly network to allow the VMs to communicate over a private network. The network that provides DHCP and internet connectivity must also be reachable from the host that is running Terraform - ensure your firewall is configured to allow this.

2. Allow Packer to infer the guest IP from ESXi without the VM needing to report it itself.

3. Open VNC ports on the firewall.

#### DetectionLab Project File Modifications

First of all, clone the repository to your workspace by `git clone git@github.com:clong/DetectionLab.git`

### Building and Deploying

After all the installation, follow the instructions [here](https://www.detectionlab.network/deployment/linuxvm/#instructions).

## Things to Notice

### Prerequisites

- It is recommended to install setup the environment using *Linux* or *macOS*. *Windows* is not recommended because some of the tools, such as *Ansible*, cannot run on *Windows* according to the [official documentation](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows).

### Building and Deploying

- SSH is commonly used when managing a remote machine. And SSH service could be installed by the following command:

  ```bash
  sudo apt install ssh
  ```

  However, due to some library conflicts, errors might occur if building the lab with SSH service enabled. It is recommended to temporarily stop the SSH service by executing the following command prior to building the lab:

  ```bash
  sudo systemctl stop ssh
  ```

  After the lab is built, the SSH service can be started by executing the following command:

  ```bash
  sudo systemctl start ssh
  ```
