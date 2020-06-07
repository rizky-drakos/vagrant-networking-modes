<h2 align="center">NAT Mode</h2>

By default NAT is automatically added when we use Vagrant to create VMs.

### Set up
Spin up 2 Ubuntu-18.04 nodes defined in the Vagrantfile.
```bash
vagrant up
```
Make sure that they are running
```bash
vagrant global-status
```
### Points to take note
:point_right: Inside your VMs, You can **access both your host machine and external networks**:
* Ping your host machine (192.168.1.6 is my host machine's internal IP address).
```bash
vagrant@node-1:~$ ping -c 2 192.168.1.6
PING 192.168.1.6 (192.168.1.6) 56(84) bytes of data.
64 bytes from 192.168.1.6: icmp_seq=1 ttl=63 time=0.364 ms
64 bytes from 192.168.1.6: icmp_seq=2 ttl=63 time=0.674 ms
```
* Ping to an external host (Google came first to my mind).
```bash
vagrant@node-1:~$ ping -c 2 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=63 time=65.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=63 time=59.1 ms
```
:point_right: Either your host machine, or external hosts cannot access these VMs.

:point_right: And different VMs cannot access each other when NAT is activated for each one.
