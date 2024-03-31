**# F5 BIG-IP Upgrade using Ansible**

### Introduction

This repo provides sample Ansible role to upgrade an F5 HA LB pair. The playbook uses modules from F5's BIG-IP Imperative Collection for Ansible  documented at [https://docs.ansible.com/ansible/latest/collections/f5networks/f5_modules/index.html](https://docs.ansible.com/ansible/latest/collections/f5networks/f5_modules/index.html).

### Features

- Pre-checks

  - Current HA status and sync check
  - Software image and availability checks
  - Restnoded and restjavad checks
  - SLB Virtual Server objects count, status check
  - Get Top 10 virtual servers based on current connections

- UCS and Qkview generation and download to local system

- Pre-upgrade tasks

  - Disable ha-group
  - Disable auto sync
  - Install software image
  - Set standby unit to offline mode

- Upgrade

  - Copy config to target volume
  - Activate volume 
  - Wait for device to reboot and become ready

- Failover Traffic to upgraded unit

- Post-upgrade tasks

  - Check if unit with new version is in standby state

  - SLB Virtual Server objects count, status check and compare with other unit

  - Enable ha-group

  - Enable auto sync

    

### Environment

Tested on a pair of BIG-IP VEs.

### Steps

- Clone repo

```
git clone git@github.com:jbollineni/f5-bigip-upgrade-ansible.git
cd f5-bigip-upgrade-ansible
```

- Create and activate python virtual environment

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

- Add upgrade pair FQDNs to `hosts.yml` file.

- Create group_vars file corresponding to the hosts in hosts.yml
- Add credentials to `all` group_vars file.

- Run playbook

```
ansible-playbook -i inventory/hosts.yml playbooks/f5_lb_upgrade.yml
```


