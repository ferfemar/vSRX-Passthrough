vSRX Passthrough
================

A simple topology to test services through a vSRX instance

Topology
========

The topology consists of three VMs: Client, Server, and vSRX. The vSRX is set in between the client and the server. Both the client and the server have appropriate routes pointing to the vSRX so they can access each other through the vSRX.

```
          +--------------------+                          
 <--------+                    |                          
      eth0|   Client           | +                        
   To host|                    | | Route to 192.168.0.0/24
          +---------+----------+ v                        
                    | eth1                                
                    | 172.16.0.10/24                      
                    |                                     
                    | 172.16.0.1/24                       
                    | ge+0/0/1.0                          
          +---------+----------+                          
 <--------+                    |                          
ge+0/0/0.0|  vSRX              |                         
   To host|                    | 
          +---------+----------+                         
                    | ge+0/0/2.0                          
                    | 192.168.0.1/24                      
                    |                                     
                    | 192.168.0.10/24                     
                    | eth1                                
          +---------+----------+ ^                        
 <--------+                    | | Route to 172.16.0.0/24 
      eth0|   Server           | +                        
   To host|                    |                          
          +--------------------+                          
```

### Vagrant Note

Do to the inner workings of Vagrant each host has a virtual NIC connected back to the host running the virtual machines. This allows Vagrant to provision and control each VM over the SSH protocol. These interfaces are depicted on the topology above.

To use this lab with VMware Fusion or Workstation you must first purchase and install the Vagrant VMware Plugin.

[Vagrant VMware Plugin](https://www.vagrantup.com/vmware)

**VM Access Information**

-	[VM Host Passwords](https://github.com/JNPRAutomate/vSRX-Passthrough/blob/master/docs/vmpasswords.md)
