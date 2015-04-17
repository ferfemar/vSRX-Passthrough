vSRX Passthrough
================

A simple topology to test services through a vSRX instance

Topology
========

The topology consists of three VMs: Client, Server, and vSRX. The vSRX is set in between the client and the server. Both the client and the server have appropriate routes pointing to the vSRX so they can access each other through the vSRX.

```
^                                        ^                                        ^
|eth0                                    |ge-0/0/0.0                              |eth0
|To host                                 |To host                                 |To host
|                                        |                                        |
++-------------------+                   ++-------------------+                   ++-------------------+
|                    |                   |                    |                   |                    |
|   Client           |eth1               |  vSRX              |               eth1|   Server           |
|                    |172.16.0.10/24     |                    |    192.168.0.10/24|                    |
|                    +-------------------+                    +-------------------+                    |
|                    |      172.16.0.1/24|                    |192.168.0.1/24     |                    |
|                    |         ge-0/0/1.0|                    |ge-0/0/2.0         |                    |
|                    |                   |                    |                   |                    |
+--------------------+                   +--------------------+                   +--------------------+
  +------------------------>                                              <------------------------+
    Route to 192.168.0.0/24                                                 Route to 172.16.0.0/24
```

### Vagrant Note

Do to the inner workings of Vagrant each host has a virtual NIC connected back to the host running the virtual machines. This allows Vagrant to provision and control each VM over the SSH protocol. These interfaces are depicted on the topology above.

**VM Access Information**

-	[VM Host Passwords](https://github.com/JNPRAutomate/vSRX-Passthrough/blob/master/docs/vmpasswords.md)
