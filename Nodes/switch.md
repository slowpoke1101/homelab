netgear gs308e managed switch
8 1gbe ports
port config:
1                2                   3       4                      5 6     7       8 
access           trunk               access  trunk                  access  access  access
port             port                port    port                   ports   port    port
vlan11           pvid11              vlan11  pvid1                  LAN     pvid99  pvid3
                 untagged 3,4,50,99          untagged 3,4,11,50,99                  
tk               sora                joe     mimi                           flint1  flint2
backup node      proxmox node        NAS     gateway/fireall                WAP     WAP
rsyslog server   docker vm           nfs,smb dns/dhcp/ids                   iot99   LAN3
snmp collector   sandbox

this node exists because i wanted to try physically separating and configuring a router, switch, and wap.
![switch ss](images/switch1.png)
![switch ss2](images/switch2.png)
