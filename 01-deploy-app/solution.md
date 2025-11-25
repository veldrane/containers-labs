### 1. Clone repo

```bash
$ git clone https://github.com/veldrane/containers-app.git
Cloning into 'containers-app'...
remote: Enumerating objects: 17, done.
remote: Counting objects: 100% (17/17), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 17 (delta 6), reused 15 (delta 4), pack-reused 0 (from 0)
Receiving objects: 100% (17/17), 7.71 KiB | 2.57 MiB/s, done.
Resolving deltas: 100% (6/6), done.
```

### 2. Build and install pinger

```bash
$ make
cargo build --release
   Compiling proc-macro2 v1.0.95
   Compiling unicode-ident v1.0.18
   Compiling fnv v1.0.7
   Compiling strsim v0.11.1
   Compiling ident_case v1.0.1
   Compiling autocfg v1.4.0
   Compiling quote v1.0.40
   Compiling serde v1.0.219
   Compiling syn v2.0.101
   Compiling num-traits v0.2.19
   Compiling serde_json v1.0.140
   Compiling ryu v1.0.20
   Compiling memchr v2.7.4
   Compiling itoa v1.0.15
   Compiling iana-time-zone v0.1.63
   Compiling darling_core v0.20.11
   Compiling serde_derive v1.0.219
   Compiling darling_macro v0.20.11
   Compiling darling v0.20.11
   Compiling serde_with_macros v3.12.0
   Compiling chrono v0.4.41
   Compiling serde_with v3.12.0
   Compiling urlencoding v2.1.3
   Compiling pinger v0.1.0 (/home/jdvorak/containers-app)
    Finished `release` profile [optimized] target(s) in 16.96s
[jdvorak@probee-1 containers-app]$ make static
cargo build --release --target=x86_64-unknown-linux-musl
   Compiling serde v1.0.219
   Compiling num-traits v0.2.19
   Compiling serde_json v1.0.140
   Compiling iana-time-zone v0.1.63
   Compiling memchr v2.7.4
   Compiling ryu v1.0.20
   Compiling itoa v1.0.15
   Compiling serde_with v3.12.0
   Compiling chrono v0.4.41
   Compiling urlencoding v2.1.3
   Compiling pinger v0.1.0 (/home/jdvorak/containers-app)
    Finished `release` profile [optimized] target(s) in 9.92s
[jdvorak@probee-1 containers-app]$ sudo make install
install -Dm755 target/release/pinger /usr/local/bin/pinger
[jdvorak@probee-1 containers-app]$ sudo make install-static
install -Dm755 target/x86_64-unknown-linux-musl/release/pinger /usr/local/bin/pinger-static
[jdvorak@probee-1 containers-app]$ strip target/x86_64-unknown-linux-musl/release/pinger
```

### 3. Enable firewalld

```bash
$ su -
Last login: Tue Jun 17 11:41:57 CEST 2025 on pts/0
[root@probee-1 ~]# 
[root@probee-1 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp1s0
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
[root@probee-1 ~]# firewall-cmd --add-port 8080/tcp --permanent
success
[root@probee-1 ~]# firewall-cmd --reload
success
```

### 4. Run the application

```bash
[root@probee-1 ~]# /usr/local/bin/pinger
2025-06-17 11:48:30 Info: Listening on 0.0.0.0:8080
```

### 5. Test it from jump server

```bash
[jdvorak@jump ~]$ curl http://probee-1:8080 -k -v
*   Trying 10.4.8.21:8080...
* Connected to probee-1 (10.4.8.21) port 8080 (#0)
> GET / HTTP/1.1
> Host: probee-1:8080
> User-Agent: curl/7.76.1
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Content-Type: text/json
* no chunk, no close, no size. Assume close to signal end
< 
* Closing connection 0
{"date":"2025-06-17 11:48:30","target":"","state":"Down","message":""}[jdvorak@jump ~]$ 
[jdvorak@jump ~]$ curl http://probee-1:8080?ip=127.0.0.1
{"date":"2025-06-17 11:48:30","target":"127.0.0.1","state":"Up","message":"PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.\n64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.109 ms\n64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.081 ms\n64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.078 ms\n\n--- 127.0.0.1 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss, time 2057ms"}
[jdvorak@jump ~]$ curl http://probee-1:8080?ip=127.0.0.1 | jq . -r
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   453    0   453    0     0    220      0 --:--:--  0:00:02 --:--:--   220
{
  "date": "2025-06-17 11:48:30",
  "target": "127.0.0.1",
  "state": "Up",
  "message": "PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.\n64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.082 ms\n64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.081 ms\n64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.078 ms\n\n--- 127.0.0.1 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss, time 2040ms\nrtt min/avg/max/mdev = 0.078/0.080/0.082/0.001 ms\n"
}
```

use jq tool for better formating