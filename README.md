# bigip-onboarding
Ansible role to automate onboarding configuration on a BIG-IP. The role will configure the following
* BIG-IP hostname
* BIG-IP SSH Banner text
* NTP servers
* DNS servers
* Provision the modules (ASM/AFM etc) on the BIG-IP
* Configure VLAN's and Self-IPs

## Requirements
* This role requires Ansible 2.4
* BIG-IP is licensed
* Packages to be installed
  - pip install f5-sdk
  - pip install bigsuds
  - pip install netaddr

## Role Variables
The variables that can be passed to this role and a brief description about them are as follows.

```
username: admin                                     //BIG-IP username
password: admin                                     //BIG-IP password

banner_text: "--Welcome to Onboarding BIGIP--"      //Banner when user SSH's into the BIG-IP

hostname: 'ansibleManaged-bigip.local'              //BIG-IP hostname

ntp_servers:                                        //NTP servers configured on BIG-IP
 - '172.27.1.1'
 - '172.27.1.2'

dns_servers:                                        //DNS servers configured on BIG-IP
 - '8.8.8.8'
 - '4.4.4.4'

dns_search_domains:                                 //DNS serach domains configured on BIG-IP
 - 'local'
 - 'localhost'

ip_version: 4                                       //DNS protocol version

vlan_information:                                   //VLAN configured on BIG-IP
 - name: 'External'                                 //Example: VLAN 'External' with VLAN tag 10
   tag: '10'                                                   tag 10 tagged to interface 1.1
   interface: '1.1'                                 
 - name: 'Internal'                                 //Example: VLAN 'Internal' with VLAN tag 11 
   tag: '11'                                                   tagged to interface 1.2
   interface: '1.2'

selfip_information:                                 //Self-IP configured on the BIG-IP
 - name: 'External-SelfIP'                                        
   address: '10.168.68.5'                                         
   netmask: '255.255.255.0'
   vlan: 'External'
   allow_service: 'default'
 - name: 'Internal-SelfIP'
   address: '192.168.68.5'
   netmask: '255.255.255.0'
   vlan: 'Internal'
   allow_service: 'default'

module_provisioning:                                 //Module provisioning on BIG-IP. 
 - name: 'asm'                                       //ASM module is enabled on the BIG-IP
   level: 'nominal'
```

## Example Playbook
```
- hosts: bigips
  gather_facts: false
  roles:
  - { role: f5devcentral.bigip-onboarding }

```

## Credential storage

Because this role includes usage of credentials to access your BIG-IP, I recommend that you supply these variables in an ansible-vault encrypted file.

This can be supplied out-of-band of this role

Steps:
- Store your vault password in a file - '~/.vault_pass.txt'
- Execute playbook as follows - ansible-vault encrypt <<variable_filename>> --vault-password-file ~/.vault_pass.txt

For more information refer to: http://docs.ansible.com/ansible/latest/playbooks_vault.html

## Certificate validation
To validate the SSL certificates of the BIG-IP REST API
- set validate_certs: true
- Generate a public private key pair
- Store the public key on BIG-IP (https://support.f5.com/csp/article/K13454#bigipsshdaccept)

## Credits
https://github.com/F5Networks/f5-ansible
