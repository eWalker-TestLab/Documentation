---
theme: prussianblue
contents:
  - Overview
  - Setup
  - Customize
  - Redeploy
  - About Terraform
  - Further Thoughts
---

# eWalker TestLab

GitHub Org: https://github.com/eWalker-TestLab

Repository: https://github.com/eWalker-TestLab/TestLab

Documentation: https://github.com/eWalker-TestLab/Documentation

---
id: 1
---

# TestLab Overview

There are 9 machines in TestLab environment. Hostnames are listed below:

- logger (collect logs from other machines)
- dc (domain controller)
- wef (Windows event forwarder)
- win10 (Windows user machine 0)
- win10user1 (Windows user machine 1)
- container0 (k8s manager node)
- container1 (k8s worker node 1)
- container2 (k8s worker node 2)
- kali (attacker)

---
layout: imagex
image: https://raw.githubusercontent.com/eWalker-TestLab/Documentation/master/img/TestLab/diagram_overview.png
pos: center
id: 1
---

---
id: 2
---

# Setup

There are 3 main steps:

- Packer build image
- Terraform apply infrastructure
- Ansible provision machines

---
layout: imagex
image: https://raw.githubusercontent.com/eWalker-TestLab/Documentation/master/img/TestLab/diagram_setup.png
pos: center
id: 2
---

---
id: 3
---

# Customize

Modify these files to customize the TestLab (simple modifications like changing MAC and IP addresses).

***Packer***:

- No need to change

***Terraform***:

- `TestLab/ESXi/variables.tf`
- `TestLab/ESXi/terraform.tfvars` (this will override the above and is recommended)

***Ansible***:

- `TestLab/ESXi/ansible/group_vars/testlab.yml`

---
id: 4
---

# Redeploy

TestLab can be redeployed multi-ways.

|     |     |
| --- | --- |
| Deploy another TestLab (under another network) | Follow the instructions in documentation and it's done. |
| Deploy another TestLab (under the same network as previous TestLab) | Modify MAC addresses in files mentioned in last slide. Then follow the instructions in documentation and it's done. |
| Replace a broken machine in TestLab | Use `terraform apply -replace` to replace the guest machine on ESXi. Then use Ansible to provision |

---
id: 5
---

# About Terraform

infrastructure as Code

## Advantages

### From my perspectives

1. Transparent
2. Easy to extend
3. Natural version control
4. Automation and time-saving
5. Community support

---
id: 5
---

# About Terraform

infrastructure as Code

## Limitations

### From my perspectives

1. Unable to take "instant" snapshots (tradeoff)
2. ESXi provider is maintained by the community

---
id: 6
---

# Further Thoughts

1. Use ***Packer*** together with ***Ansible*** in the first stage (further reduce rebuild time)

---
layout: imagex
image: https://raw.githubusercontent.com/eWalker-TestLab/Documentation/master/img/TestLab/diagram_setup_future.png
pos: center
id: 6
---
