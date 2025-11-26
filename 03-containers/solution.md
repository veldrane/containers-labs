### Information about containers

```
# docker ps
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
CONTAINER ID  IMAGE                                               COMMAND     CREATED        STATUS        PORTS       NAMES
2198de0f2a3b  registry.class.syscallx86.com/jdvorak/pinger:0.5.0              2 seconds ago  Up 2 seconds              pinger
# docker inspect 2198de0f2a3b | grep -i pid
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
               "Pid": 1806,
               "ConmonPid": 1804,
          "ConmonPidFile": "/run/containers/storage/overlay-containers/2198de0f2a3bf075ac20f9ddeac43751cd4d4e707a5ca2acdeebd2bd6192b6bd/userdata/conmon.pid",
          "PidFile": "/run/containers/storage/overlay-containers/2198de0f2a3bf075ac20f9ddeac43751cd4d4e707a5ca2acdeebd2bd6192b6bd/userdata/pidfile",
               "PidMode": "private",
               "PidsLimit": 2048,
```

### Usage host tools in container

```
# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 96:82:fc:fc:8c:84 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.88.0.2/16 brd 10.88.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::9482:fcff:fefc:8c84/64 scope link 
       valid_lft forever preferred_lft forever
```

### Change context of the container

```
# ps -fe
-bash: ps: command not found
[root@2198de0f2a3b /]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 96:82:fc:fc:8c:84 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.88.0.2/16 brd 10.88.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::9482:fcff:fefc:8c84/64 scope link 
       valid_lft forever preferred_lft forever
[root@2198de0f2a3b /]# id
uid=0(root) gid=0(root) groups=0(root)
[root@2198de0f2a3b /]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
[root@2198de0f2a3b /]# df
Filesystem     1K-blocks   Used Available Use% Mounted on
overlay          6119008 407964   5422036   7% /
tmpfs              65536      0     65536   0% /dev
tmpfs             748680  17148    731532   3% /etc/hosts
shm                64000      0     64000   0% /dev/shm
devtmpfs            4096      0      4096   0% /proc/keys
[root@2198de0f2a3b /]# pwd
/
[root@2198de0f2a3b /]# ls -lta
total 728
drwxr-xr-x.   5 root root    340 Nov 25 14:02 dev
drwxr-xr-x.   1 root root   4096 Nov 25 14:02 run
dr-xr-xr-x. 222 root root      0 Nov 25 14:02 proc
dr-xr-xr-x.   1 root root   4096 Nov 25 14:02 .
dr-xr-xr-x.   1 root root   4096 Nov 25 14:02 ..
dr-xr-xr-x.  13 root root      0 Nov 25 13:08 sys
drwxr-xr-x.   1 root root   4096 Nov 25 11:50 etc
dr-xr-x---.   1 root root   4096 Nov 25 11:50 root
-rwxr-xr-x.   1 root root 668208 Nov 25 11:49 pinger-static
drwxr-xr-x.   1 root root   4096 Nov 19  2023 var
drwxr-xr-x.   1 root root   4096 Nov 19  2023 usr
drwxrwxrwt.   2 root root   4096 Nov 19  2023 tmp
drwx------.   2 root root   4096 Nov 19  2023 lost+found
dr-xr-xr-x.   2 root root   4096 May 16  2022 afs
lrwxrwxrwx.   1 root root      7 May 16  2022 bin -> usr/bin
drwxr-xr-x.   2 root root   4096 May 16  2022 home
lrwxrwxrwx.   1 root root      7 May 16  2022 lib -> usr/lib
lrwxrwxrwx.   1 root root      9 May 16  2022 lib64 -> usr/lib64
drwxr-xr-x.   2 root root   4096 May 16  2022 media
drwxr-xr-x.   2 root root   4096 May 16  2022 mnt
drwxr-xr-x.   2 root root   4096 May 16  2022 opt
lrwxrwxrwx.   1 root root      8 May 16  2022 sbin -> usr/sbin
drwxr-xr-x.   2 root root   4096 May 16  2022 srv
[root@2198de0f2a3b /]# pwd
/
[root@2198de0f2a3b /]# 
```

### Look at inside container and namespaces via nsenter command (will be discussed next chapter)
```
[root@probee-1 build]# nsenter -t 1806 -p -m -- ps -fe
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 14:28 ?        00:00:00 /pinger-static
root           2       0  0 14:28 ?        00:00:00 ps -fe
```