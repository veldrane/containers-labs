## Make a root from scratch

```
mkdir /opt/pinger-rootfs
dnf install --releasever=9 --installroot=/opt/pinger-rootfs --setopt=install_weak_deps=False --nodocs -y passwd shadow-utils bash net-tools iproute curl vim-minimal nano
```

## Prepare veth pars on the host machine

Please replace "x" your assigned subnets.

### Add network ns

```bash
ip netns add pinger
ip netns list
```

### Add veth pair


```bash
ip link add veth-host type veth peer name veth-pinger
ip link set veth-pinger netns pinger
```

Create veth pair with two network devices: one is named veth-host, second veth-pinger. Check if you see both of them:

```bash
# ifconfig -a

# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:01:8c:6f brd ff:ff:ff:ff:ff:ff
    inet 10.4.0.21/24 brd 10.4.0.255 scope global noprefixroute enp1s0
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe01:8c6f/64 scope link 
       valid_lft forever preferred_lft forever
3: veth-pinger@veth-host: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether a6:d2:e1:9b:b5:76 brd ff:ff:ff:ff:ff:ff
4: veth-host@veth-pinger: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 6a:d5:65:1e:de:70 brd ff:ff:ff:ff:ff:ff
```

### Configure veth pair on the host network namespace

```bash
ip addr add 10.200.x.1/24 dev veth-host
ip link set veth-host up
```


### Configure veth pair on the container namespace


```bash
ip netns exec pinger ip addr add 10.200.x.2/24 dev veth-pinger
ip netns exec pinger ip link set veth-pinger up
ip netns exec pinger ip link set lo up
```


### Complete network cfg
```bash
ip netns add pinger
ip netns list
ip link add veth-host type veth peer name veth-pinger
ip link set veth-pinger netns pinger
ip addr add 10.200.x.1/24 dev veth-host
ip link set veth-host up
ip netns exec pinger ip addr add 10.200.0.2/24 dev veth-pinger
ip netns exec pinger ip link set veth-pinger up
ip netns exec pinger ip link set lo up
```


## Start pinger in own namespace


### Run the ip net ns command with unshare :)

```bash
# ip netns exec pinger unshare --fork --pid --mount --mount-proc --uts --ipc -- chroot /opt/pinger-rootfs/ /bin/bash
bash-5.1#
```


### Optiona docker tasks

# lsns | grep net
4026531840 net       156     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 30
4026532493 net         1   827 root   /usr/sbin/irqbalance
4026532568 net         1  6759 root   /pinger-static

# nsenter -n -t 6759
[root@probeeh docker-build]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0@if12: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 56:36:91:4f:63:ac brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.88.0.6/16 brd 10.88.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::5436:91ff:fe4f:63ac/64 scope link 
       valid_lft forever preferred_lft forever