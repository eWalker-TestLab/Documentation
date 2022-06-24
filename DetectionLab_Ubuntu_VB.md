
# Ubuntu 20.04 + VirtualBox 6.1

## Build Procedure

1. Before starting building the lab, updating the system is recommended. This can be done by executing `sudo apt update` and then `sudo apt full-upgrade -y`. Finally, `reboot` the machine.

2. Install necessary tools by executing the following command:

   ```bash
   sudo apt install build-essential curl git
   ```

3. Install *VirtualBox* and *extension packs* by executing the following command:

   ```bash
   sudo apt install virtualbox virtualbox-ext-pack
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

6. After all the installation, follow the instructions [here](https://www.detectionlab.network/deployment/linuxvm/#instructions)

## Things to Notice

- SSH is commonly used when managing a remote machine. And SSH service could be installed by the following command:

  ```bash
  sudo apt install ssh
  ```

  However, errors might occur if building the lab with SSH service enabled due to some library conflicts. It is recommended to temporarily stop the SSH service prior to building the lab by executing the following command:

  ```bash
  sudo systemctl stop ssh
  ```

  After the lab is built, the SSH service can be started by executing the following command:

  ```bash
  sudo systemctl start ssh
  ```