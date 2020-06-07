<h3 align="center">Internal Networking</h3>

Choosing VirtualBox as the provider for Vagrant, we can enable internal networks. However,
NAT is a compulsory network created by Vagrant, so as to correctly build a internal 
network with Vagrant, there must be manual configuration steps involved.

### Set up
*Goal:* Create two VMs, in that they are both connected to the same internal network. However 
we cannot access to internal network VMs without one of the VMs connected to a NAT, we will choose 
node-1 for that.

*Steps:*

Spin up 2 Ubuntu-18.04 nodes defined in the Vagrantfile.
```bash
vagrant up
```
Make sure they are running.
```bash
vagrant global-status
```
SSH into either of the two VMs.
```bash
vagrant ssh node-1
```
Continue to SSH into the other VM.
```bash
ssh vagrant@10.0.0.12
```
> Note: 10.0.0.12 is the static IP of the node-2, it was set in the Vagrantfile. *vagrant* is 
the default password.

And delete its NAT, which is created automatically be Vagrant.
```bash
sudo ifconfig eth0 down
```
> Note: At this point, there is no way we can access into node-2 from hosts outside of its 
internal network but an indirect way via node-1.
### Points to take note
> Note: All of the commands following should be executed inside node-2, because node-1 has a NAT 
connection which is not considered to be purely connected to internal network.

:point_right: In contrast to NAT, inside internal network VMs you **cannot access either your host** 
**machine or external hosts**.
* Ping your host machine (**192.168.1.6** is my host machine's internal IP address). 
```bash
vagrant@node-2:~$ ping 192.168.1.6
connect: Network is unreachable
```
* Ping to an external host (Google came first to my mind).
```bash
vagrant@node-2:~$ ping 8.8.8.8
connect: Network is unreachable
```
:point_right: VMs **in the same internal network is able to talk to each other**.
* Ping node-1, standing inside node-2.
```bash
vagrant@node-2:~$ ping -c 2 10.0.0.11
PING 10.0.0.11 (10.0.0.11) 56(84) bytes of data.
64 bytes from 10.0.0.11: icmp_seq=1 ttl=64 time=0.623 ms
64 bytes from 10.0.0.11: icmp_seq=2 ttl=64 time=0.786 ms
```
> Note: 10.0.0.11 is the static IP of node-1.
* Ping node-2, standing inside node-1.
```bash
vagrant@node-1:~$ ping 10.0.0.12
PING 10.0.0.12 (10.0.0.12) 56(84) bytes of data.
64 bytes from 10.0.0.12: icmp_seq=1 ttl=64 time=0.194 ms
64 bytes from 10.0.0.12: icmp_seq=2 ttl=64 time=0.746 ms
```
:point_right: Internal network VMs cannot be accessed from hosts which are not in 
their internal network, this includes your local host machines, external hosts and 
hosts connecting to your localhost's physical bridged network.
