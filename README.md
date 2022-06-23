# TestLab Documentation

## DetectionLab Setup

This section introduces how to set up [DetectionLab](https://github.com/clong/DetectionLab) on your machine. Select the corresponding environment for your machine:

### Windows 10 + VMware Workstation Pro 16

#### Build Procedure

1. Install *VMware Workstation Pro 16* from [the official website](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html).
2. Install *Vagrant* from [the official website](https://www.vagrantup.com/downloads).
3. Install *Vagrant VMware Utility* from [the official website](https://www.vagrantup.com/vmware/downloads).
4. Install *Vagrant VMware provider plugin* by executing the command:

   `$ vagrant plugin install vagrant-vmware-desktop`

5. Follow the instructions [here](https://www.detectionlab.network/deployment/windowsvm/#instructions).

#### Things to Notice

1. When executing the `.\prepare.ps1` script, ensure your machine's execution policy is correct. Refer to [the official documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?preserve-view=true&view=powershell-7.2&viewFallbackFrom=powershell-7.1). Alternatively, temporarily bypass the policy by adding the `-ep Bypass` flag.
2. If you already have multiple virtual machine platforms installed on your machine, please examine the *network adapter settings* of your machine in order to avoid any collision. For example, you may want to check whether `192.168.56.1` is occupied or not because DetectionLab will utilize this address.
3. Outputting debug information is highly recommended. This can be done by adding the `--debug 2>&1 | Tee-Object -FilePath "PATH TO YOUR LOG FILE"` flag after each `vagrant` command. Refer to [the official documentation](https://www.vagrantup.com/docs/other/debugging).
4. When building **dc**, Vagrant might complain about "Timed out while waiting for the machine to boot" as described [here](https://github.com/clong/DetectionLab/issues/827). If you encounter this error but observe your **dc** VM is indeed up and running, you might want to change `timeout` settings in `Vagrantfile` under `DetectionLab/Vagrant` directory. A working practice is to set `timeout` to `2400`.
5. When building Windows VMs (i.e., **dc**, **wef**, **win10**), *VMware Tools* might not be installed correctly, causing the failure of enabling shared folder, as described [here](https://github.com/clong/DetectionLab/issues/720). You should manually install *VMware Tools* inside those VMs. After installation, run the command `vagrant reload <VM name>`.

### Ubuntu 20.04 + VrtualBox 6.1.34

#### Build Procedure

#### Things to Notice

## BlueTeam.Lab Setup

This section introduces how to set up [BlueTeam.Lab](https://github.com/op7ic/BlueTeam.Lab) on your machine. Select the corresponding environment for your machine:
