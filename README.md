Arista-Lab-PC


Autor: 	Galo Montes
Date:	31/5/2017

Description:
This is a lab to text how to administrate Arista EOS switches in DevOps style.

I have add a local "hosts" file to avoid to go to /etc/ansible/hosts. in this way, now i have to execute "ansible-playbook -i hosts xxx.yaml"

The -i options means the file where it is the host_file.

Created a file "hostsAristaDCL2" correspondiente a DC Arista L2. 

################## hostsAristaDCL2 <start> ########################
# hosts file
[spines]
spine-sw1
spine-sw2

[leafs]
leaf-sw1
leaf-sw2
leaf-sw3
leaf-sw4

################## hostsAristaDCL2 <end> ########################

Change /etc/hosts to avoid to change DNS in the firewall.

10.10.101.20	spine-sw1
10.10.101.21	spine-sw2
10.10.101.10	leaf-sw1
10.10.101.11	leaf-sw2
10.10.101.11	leaf-sw3
10.10.101.11	leaf-sw4

Test with pings that all the switches are reachable. 

Create a file structure for Arista DC L2 type.
- A main file "AristaDCL2.yml" that call other subfiles.
	- leafs.yaml for leafs switches roles.
	- spines.yaml for spines switches roles.

################## AristaDCL2.yml <start> ########################
#Playbook for Arista DC L2 type.
- include: spine.yaml
- include: leaf.yaml
################## AristaDCL2.yml <end> ########################

################## leafs.yaml<start> ########################
# Playbook for leafs switches
- hosts: leafs
  gather_facts: no

  roles:
        - arista.eos-bridging
        - arista.eos-interfaces
        - arista.eos-ipv4
        - arista.eos-mlag
        - arista.eos-system
################## leafs.yaml <end> ########################

################## spines.yaml<start> ########################
# Playbook for leafs switches
- hosts: spines
  gather_facts: no

  roles:
        - arista.eos-bridging
        - arista.eos-interfaces
        - arista.eos-ipv4
        - arista.eos-mlag
        - arista.eos-system
        - arista.eos-virtual-router
	- arista.eos-vxlan
        - arista.eos-route-control
        - arista.eos-bgp
################## spines.yaml <end> ########################

Created a file to provide a centrel reference credentials for authentication to all the switches in group_vars/all.yaml

################## all.yaml <start> ########################
---
# File to authenticate all the switches with the same directory
ansible_python_interpreter: python
provider:
  host: "{{ inventory_hostname }}"
  username: admin
  password: digital1
  authorize: yes
  transport: cli
################## all.yaml <end> ########################

To test the connection, run the command 
	#ansible leafs -i hostsAristaDCL2 -m ping
