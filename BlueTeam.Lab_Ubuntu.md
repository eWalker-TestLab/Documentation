# Ubuntu 20.04 in VMware Workstation Pro 16

Although this setup is done using guest OS in VMware, the same should apply to native OS running bare metal.

## Build Procedure

### Prerequisites

Some additional steps are recommended besides the original instructions [here](https://github.com/op7ic/BlueTeam.Lab#prerequisites). Thus, please use the following steps:

1. Install *Azure CLI* using the following command. More details can be found in the [official documentation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt).

   ```bash
   curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
   ```

2. Install *Terraform* using the following commands. More details can be found in the [official documentation](https://learn.hashicorp.com/tutorials/terraform/install-cli).

    ```bash
    sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
    sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
    sudo apt-get update && sudo apt-get install terraform
    ```

3. Install *Ansible* using the following commands. More details can be found in the [official documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

   ```bash
   sudo apt update
   sudo apt install software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt update
   sudo apt install ansible
   ```

4. Install *Python* and *pip3* using the following command.

   ```bash
   sudo apt install python3 python3-pip
   ```

5. Upgrade some packages required by the toolchain using the following command.

   ```bash
   python3 -m pip install --upgrade cryptography PyJWT
   ```

6. Finally, install python and various packages needed for remote connections and other activities using the following command.

   ```bash
   python3 -m pip install pywinrm requests msrest msrestazure azure-cli
   ```

### Building and Deploying

## Things to Notice

### Prerequisites

- It is recommended to install setup the environment using supported *Linux* distributions listed [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux). *Windows* is not recommended because some of the tools, such as *Ansible*, cannot run on *Windows* according to the [official documentation](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows)

- After installing packages using `pip3`, `pip3` may complain about the following:

  ```log
  WARNING: The scripts xxx are installed in '~/.local/bin' which is not on PATH.
  ```

  Under this situation, you could add the path in the warning message to your PATH. For example, add the following line to the end of the `~/.bashrc` file.

  ```bash
  export PATH="~/.local/bin:$PATH"
  ```

### Building and Deploying

- Outputting the *Terraform* debug info is highly recommended, which the following commands can achieve. More details can be found in the [official documentation](https://www.terraform.io/internals/debugging)

  ```bash
  export TF_LOG="DEBUG"
  export TF_LOG_PATH="path to your log file"
  ```
