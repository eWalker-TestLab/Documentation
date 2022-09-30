# TODO 

- filebeat
   - [ ] install filebeat on container / docker
   - [ ] modify logstash config to receive log from filebeat

- windows domain name 
   - [ ] make domain name variable (current: windows.domain / prosteritas.local) 

- winlogbeat config
   - [x] set **logger** ipaddress variable on **common** winlogbeat config file with "win_lininfile" command 

- wazuh
   - [ ] make wazuh sever send alert log to wazuh dashboard
   - [ ] check wazuh dashboard logging
   - [ ] make logstash send log to wazuh
   - [ ] make wazuh agent send event log to logstash
   - [ ] check wazuh / opensearch dashboard for the log outcome

# the following come from Q&A session 
- [x] How to upgrade OS system with packer/ansible? (e.g. win10 -> win11 / e.g. hotfix/security patch) 
      The virtual machine should be able to stand alone. That means they could always update with the windows hotfix/security patch. User also could modify Windows Update service as they want. 

- [x] Can we copy file(upload script) in packer?
      Yes, with the use of packer (File Provisioner)[https://www.packer.io/docs/provisioners/file].

- [x] What happen if packer script have undefined variable?
      We could espect the undefined variables in packer json script to be empty string. The (Packer JSON Template)[https://www.packer.io/docs/templates/legacy_json_templates/user-variables] suggests "Even if you want a user variable to default to an empty string, it is best to explicitly define it". The (Packer HCL Template)[https://www.packer.io/guides/hcl/variables] also suggests "if a variable was never defined it would generally be interpolated to an empty string". 

- [ ] can hardcoded password in preseed.cfg file used in ubuntu_2004_esxi.json be variable? or run a script to build preseed.cfg?
   (the existing script ping for preseed.cfg on detectionlab)

- [x] Is it possible build template with 'thin provision'? make "boot_disk_type = thick" be "thin" instead?
      Refering to (VMware Builder)[https://www.packer.io/plugins/builders/vmware/iso#disk_type_id], thin provision should be possible. 

- [ ] is hardcoded mac address necessary

- [~] Does opensearch apt package exsit?
      Not in ubuntu 2004

- [ ] how to keep data even with terraform destroy 

- [ ] Use hostname instead of ip address in config (dc already provides DNS service)

- [ ] ansible inventory implement group
