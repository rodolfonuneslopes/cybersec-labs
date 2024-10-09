# Wazuh all-in-one @ AWS
It's the simplest way to learn and test Wazuh, since all components run in the same machine. This guide explains how to implement wazuh at AWS.
## Requirements
### Supported Operating Systems
- Amazon Linux 2
- CentOS 7, 8
- Red Hat Enterprise Linux 7, 8, 9
- Ubuntu 16.04, 18.04, 20.04, 22.04
### Hardware resources
- 2 CPUs 
- 4GB RAM
- 50GB HDD/SSD
## Configure Amazon Linux 2 (AL2) in VirtualBox
Amazon's guidelines advise to use a provided Virtual Disk Image (.vdi) and a `Seed.iso` to hold the configurations (users etc.). In VirtualBox:
### 1. Create a new virtual machine with no .iso
![](Pasted%20image%2020240820150006.png)
### 2. Select existing Virtual Disk and Add New Disk
![](Pasted%20image%2020240820150639.png)
![](Pasted%20image%2020240820150829.png)
### 3. Navigate to the downloaded `.vmi` and choose it
![](Pasted%20image%2020240820151120.png)
![](Pasted%20image%2020240820151235.png)

### 4. Add `Seed.iso` as disk file
![](Pasted%20image%2020240820152002.png)
![](Pasted%20image%2020240820152115.png)
### Resources
- [amzn2-virtualbox-2.0.20190612-x86_64.xfs.gpt.vdi](https://cdn.amazonlinux.com/os-images/2.0.20190612/virtualbox/amzn2-virtualbox-2.0.20190612-x86_64.xfs.gpt.vdi)
- [Seed.iso](https://cdn.amazonlinux.com/os-images/2.0.20190612/Seed.iso)
## Configure AL2 in AWS
The simplest way to deploy an Amazon Linux instance in AWS is to use an AMI
![](Pasted%20image%2020240820153859.png)
## Install Wazuh Server
```Shell
curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```
## Set user-friendly admin password
### Download Wazuh Password Tool
```shell
curl -so wazuh-passwords-tool.sh https://packages.wazuh.com/4.7/wazuh-passwords-tool.sh
```
### Set admin password to `Wazuh.1234`
```shell
sudo bash wazuh-passwords-tool.sh -u admin -p Wazuh.1234
```
## Install Wazuh Agent
The best way to deploy agents is to follow the step-by-step in the Wazuh Dashboard:
![](Pasted%20image%2020240820154541.png)
### Template for Ubuntu Server 24
``` Shell
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.8.1-1_amd64.deb && sudo WAZUH_MANAGER='<IP address/domain name>' WAZUH_AGENT_NAME='ubuntu-server-agent' dpkg -i ./wazuh-agent_4.8.1-1_amd64.deb
```

``` Shell
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```