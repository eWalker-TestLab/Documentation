# VirtualBox 6.1 on Ubuntu 20.04

- [VirtualBox 6.1 on Ubuntu 20.04](#virtualbox-61-on-ubuntu-2004)
   - [Prerequisites](#prerequisites)
   - [Build and Deploy](#build-and-deploy)
   - [Things to Notice](#things-to-notice)
      - [Prerequisites](#prerequisites-1)
      - [Build and Deploy](#build-and-deploy-1)

## Prerequisites

1. Before starting building the lab, updating the system is recommended. This can be done by executing `sudo apt update` and then `sudo apt full-upgrade -y`. Finally, `reboot` the machine.

2. Install necessary tools by executing the following command:

   ```shell
   sudo apt install build-essential curl git gnupg software-properties-common
   ```

3. Install *VirtualBox* and *extension packs* by executing the following command:

   ```shell
   sudo apt install virtualbox virtualbox-ext-pack
   ```

4. Install *Vagrant* by executing the following commands (more info on [the official website](https://www.vagrantup.com/downloads)):

   ```shell
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install vagrant
   ```

5. Install *Packer* by executing the following commands (more info on [the official website](https://www.packer.io/downloads)):

   ```shell
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install packer
   ```

## Build and Deploy

After all the installation, follow the instructions [here](https://www.detectionlab.network/deployment/linuxvm/#instructions).

## Things to Notice

### Prerequisites

- Building the lab on a low-end machine is not recommended because manual configurations might be required under this condition due to the slow running speed of the machine. For example, when the *network visibility* option prompts, you may have to configure it manually. Also, when the login page appears, you may have to manually enter the password. Credentials can be found [here](https://www.detectionlab.network/introduction/infoandcreds/).

### Build and Deploy

- Outputting *Vagrant* debug information is highly recommended. This can be done by adding the `--debug &> <PATH TO YOUR LOG FILE>` flag after each `vagrant` command. For example, `vagrant up --debug &> ~/vagrant_up.log`. Refer to [the official documentation](https://www.vagrantup.com/docs/other/debugging).

- SSH is commonly used when managing a remote machine. However, due to some library conflicts, errors might occur if building the lab with SSH service enabled. It is recommended to temporarily stop the SSH service by executing the following command prior to building the lab:

  ```shell
  sudo systemctl stop ssh
  ```

  After the lab is built, the SSH service can be started again by executing the following command:

  ```shell
  sudo systemctl start ssh
  ```
