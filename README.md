# bigip-onbording
Ansible role to automate onboarding configuration on a BIG-IP

## Requirements
* This role requires Ansible 2.4
* BIG-IP is licensed

## Role Variables
The variables that can be passed to this role and a brief description about them are as follows.

username: admin                                                   //BIG-IP username
password: admin                                                   //BIG-IP password

banner_text: "--------Welcome to Onboarding BIGIP----------"      //Banner when user SSH's into the BIG-IP

hostname: 'ansibleManaged-bigip.local'                            //BIG-IP hostname

ntp_servers:                                                      //NTP servers configured on BIG-IP
 - '172.27.1.1'
 - '172.27.1.2'

dns_servers:                                                      //DNS servers configured on BIG-IP
 - '8.8.8.8'
 - '4.4.4.4'

dns_search_domains:                                               //DNS serach domains configured on BIG-IP
 - 'local'
 - 'localhost'

ip_version: 4                                                     //DNS protocol version

vlan_information:                                                 //VLAN configured on BIG-IP
 - name: 'External'                                               //Example: VLAN 'External' with VLAN tag 10 (tagged to interface 1.1)
   tag: '10'
   interface: '1.1'
 - name: 'Internal'                                               //Example: VLAN 'Internal' with VLAN tag 11 (tagged to interface 1.2)
   tag: '11'
   interface: '1.2'

selfip_information:                                               //Self-IP configured on the BIG-IP
 - name: 'External-SelfIP'                                        //
   address: '10.168.68.5'                                         
   netmask: '255.255.255.0'
   vlan: 'External'
   allow_service: 'default'
 - name: 'Internal-SelfIP'
   address: '192.168.68.5'
   netmask: '255.255.255.0'
   vlan: 'Internal'
   allow_service: 'default'

module_provisioning:
 - name: 'asm'
   level: 'nominal'



