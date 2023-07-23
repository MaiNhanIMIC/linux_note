<h1>Get internet for beaglebone</h1>
<p>the first configurate a bridge to share internet from network interface of laptop to beaglebone</p>
<h2>Config gateway</h2>
run command in beaglebone:

```
sudo route add default gw 192.168.7.1
```
config DNS server, open file ```/etc/resolv.conf``` with root permission add command to the file:

```
nameserver 8.8.8.8
```

check with command to ping to google.com:

```
ping google.com
```
if dont work we can reset network service by command:

```
sudo service networking restart
```

yeah.! enjoy