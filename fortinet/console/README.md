![Fortinet logo|](https://upload.wikimedia.org/wikipedia/commons/thumb/6/62/Fortinet_logo.svg/320px-Fortinet_logo.svg.png)

## Fortinet Console Ansible Collection
***

The collection is the Fortinet Console Ansible Automation project. It includes the modules that are able to access and communicate Fortinet devices through remote console server.

## Installation
This collection is distributed via [ansible-galaxy](https://galaxy.ansible.com/), the installation steps are as follows:

1. Install or upgrade to Ansible 2.9+
2. Download this collection from galaxy: `ansible-galaxy collection install fortinet.console:0.2.2`

## Requirements
* Ansible 2.9+ is required to support the newer Ansible Collections format

## Tested devices
This collection has been tested with:
Remote Console Server:
  MRV LX-4032:
    test in my home lab
  Avocent ACS 8000:
    test with my customer which has ACS 8000 in production data center
  FortiPoC:
    A Fortinet internal tool which is KVM based to run VMs for demos or POCs
    FortiPoC is not a remote console server, but it provides the virtual console access to VMs running on it
    So I can techically test/use my Fortinet Console modules with VMs and virtual console ports
    To do that, playbook will do 3 steps access,
    1) step 1, ssh to FortiPoC
    2) step 2, on FortiPoC cli, run "poc device console NAMW_OF_MY_GUEST_VM", this is the become option in Fortinet Console modules for
    3) step 3, login to "GUEST_VM", which could be FGT/FMG/FAZ VM.

FortiGate:
  FortiGate-501E, FOS version 6.0/6.2/6.4
  FortiGate-VM (KVM), FOS version 6.0/6.2/6.4

FortiManager:
  FortiManager-VM (KVM), FMG version 6.0/6.2/6.4


__Note__: Use `-f` option (i.e. `ansible-galaxy collection install -f fortinet.console:x.x.x`) to renew your existing local installation.


## Modules
The collection provides the following modules:


* `fortigate_remote_console` Access FortiGate through remote console server.
* `Fortimanager_remote_console` Access FortiManager/FortiAnalyzer through remote console server.

## Roles


## Usage
The following example is used to factory reset FortiGate through remote console access.

Create `fortigate_factoryreset.yml` with the following template:
```yaml
---
- name: Fortinet Console Example Playbook - User Remote Console Server to Factory Reset FortiGate Firewall
  hosts: fortigate01
  collections:
    - fortinet.console
  tasks:
  - name: With remote console access, factory reset FGT
    fortigate_remote_console:
      rcs_ip: "{{ term_server }}"
      rcs_username: "{{ term_user }}"
      rcs_password: "{{ term_password }}"
      rcs_fgt_port: "{{ term_ssh_port }}"
      rcs_fgt_username: "{{ dev_user }}"
      rcs_fgt_password: "{{ dev_password }}"
      rcs_fgt_become: "{{ term_become|default(omit) }}"
      rcs_fgt_action: "factoryreset"
  register: fortigate_remote_console_result
  - debug:
    msg: "{{ fortigate_remote_console_result }}"
```
Create the `hosts` inventory file
```
[fortigates]
fortigate01 term_server=192.168.190.130 term_ssh_port="2922" term_user="InReach" term_password="access" dev_user="admin" dev_password="password"

```

Run the test:
```bash
ansible-playbook -i hosts fortigate_factoryreset.yml
```

This will factory reset FortiGate appliance.