<h2 align="center">NAT Mode</h2>
<h3>Set up</h3>
<p>Spin up 2 Ubuntu-18.04 nodes</p>
```bash
vagrant up
```
<p>Make sure that they are running</p>
```bash
vagrant global-status
```
<h3>Experiment</h3>
Ping your host machine (192.168.1.6 is my host machine's internal IP address)
```bash
ping 192.168.1.6
```
Ping to an external host (Google came first to my mind)
```bash
ping 8.8.8.8
```