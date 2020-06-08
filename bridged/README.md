<h2 align="center">Bridged Networking</h2>

### Set up
*Goal:* Create two VMs, and **connect them to the physical network of the host machine**. This  
is the Bridged mode. Definitely to make them look more like being solely connected to a 
bridged network we need to manually remove NAT from all of them.

*Steps:*

Spin up 2 Ubuntu-18.04 nodes defined in the Vagrantfile.
```bash
vagrant up
```
Make sure they are running.
```bash
vagrant global-status
```
SSH into the node-1.
> Note: We SSH into these VMs via their static IPs set in the Vagrantfile instead of 
running *vagrant ssh <node-name>*. The reason is that we need to remove their NATs and
your console will get hung when vagrant performs SSH via the ANT. And by default *vagrant* 
is the default password.
```bash
ssh vagrant@"192.168.1.11
```
Remove its NAT, which is eth0 by default.
```bash
sudo ifconfig eth0 down
```
Repeat these steps for node-2

At this point, your VMs are fully connected to a bridged network.

### Points to take note
:point_right: Your VMs using the same physical network as your local host does, which means 
they share the same access policy as the local host.

:point_right: They can ping each other, and they can ping your local host too.