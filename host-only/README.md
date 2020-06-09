### Set up
*Goal:* Create two VMs, and connect them to a host-only adapter. Because NAT is added to each VM 
by Vagrant, we will manually access into each VM and remove the NAT adapter to correctly create 
a host-only network.

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
```bash
ssh vagrant@10.0.0.11
```
> Note: 10.0.0.11 is the static IP address of node-1 that was set in the Vagrantfile
Remove its NAT, which is eth0 by default.
```bash
sudo ifconfig eth0 down
```
Repeat these steps for node-2

At this point, your VMs are fully connected to a host-only network.

### Points to take note
:point_right: In a host-only network, you can see your local host machine together with all VMs 
as a group. With that being said, they can ping each other.
* Ping your host machine (**192.168.1.6** is my host machine's internal IP address).
```bash
vagrant@node-1:~$ ping -c 2 192.168.1.6
PING 192.168.1.6 (192.168.1.6) 56(84) bytes of data.
64 bytes from 192.168.1.6: icmp_seq=1 ttl=63 time=0.364 ms
64 bytes from 192.168.1.6: icmp_seq=2 ttl=63 time=0.674 ms
```
* Ping either of VMs.
```bash
rizky@rizky-machine:~$ ping -c 2 10.0.0.11
PING 10.0.0.11 (10.0.0.11) 56(84) bytes of data.
64 bytes from 10.0.0.11: icmp_seq=1 ttl=64 time=0.385 ms
64 bytes from 10.0.0.11: icmp_seq=2 ttl=64 time=0.485 ms
```
:point_right: Nonetheless VMs cannot access external hosts, and vice versa.
