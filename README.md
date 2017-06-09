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