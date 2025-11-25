### Simple Dockerfile

```Dockerfile
FROM ustclug/rocky:9-minimal

LABEL MANTAINER "-veldrane"

COPY pinger-static /

RUN microdnf install -y net-tools iproute iputils bash \
    && chmod +x /pinger-static

ENTRYPOINT ["/pinger-static"]
```


### Docker/Podman install (pod rootem)

```bash
# dnf -y install docker
```

### Build docker image (pod rootem)

```bash
$ cp pinger pinger-static
[jdvorak@probee-1 build]$ sudo docker build -t registry.class.syscallx86.com/jdvorak/pinger:0.5.0 .
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
[2/2] STEP 1/5: FROM ustclug/rocky:9-minimal
[2/2] STEP 2/5: LABEL MANTAINER "jdvorak"
--> Using cache 2a41c0b85d6474c6537b9bcfa4199c79bfd9d5156815c1c69fdd74f42ed85bd9
--> 2a41c0b85d64
[2/2] STEP 3/5: COPY pinger-static /
--> 1c773e703d7e
[2/2] STEP 4/5: RUN microdnf install -y net-tools iproute iputils bash     && chmod +x /pinger-static
Downloading metadata...
Downloading metadata...
Downloading metadata...
Package                                             Repository     Size
Installing:                                                            
 acl-2.3.1-4.el9.x86_64                             baseos      70.9 kB
 dbus-1:1.12.20-8.el9.x86_64                        baseos       7.0 kB
 dbus-broker-28-7.el9.x86_64                        baseos     175.0 kB
 dbus-common-1:1.12.20-8.el9.noarch                 baseos      13.9 kB
 elfutils-libelf-0.192-6.el9_6.x86_64               baseos     207.8 kB
 expat-2.5.0-5.el9_6.x86_64                         baseos     117.6 kB
 iproute-6.11.0-1.el9.x86_64                        baseos     820.3 kB
 iputils-20210202-11.el9_6.3.x86_64                 baseos     171.3 kB
 kmod-libs-28-10.el9.x86_64                         baseos      63.9 kB
 libbpf-2:1.5.0-1.el9.x86_64                        baseos     189.6 kB
 libmnl-1.0.4-16.el9_4.x86_64                       baseos      27.8 kB
 libseccomp-2.5.2-2.el9.x86_64                      baseos      72.3 kB
 net-tools-2.0-0.64.20160912git.el9.x86_64          baseos     301.3 kB
 psmisc-23.4-3.el9.x86_64                           baseos     231.6 kB
 systemd-252-51.el9_6.3.rocky.0.1.x86_64            baseos       4.2 MB
 systemd-pam-252-51.el9_6.3.rocky.0.1.x86_64        baseos     280.9 kB
 systemd-rpm-macros-252-51.el9_6.3.rocky.0.1.noarch baseos      63.7 kB
Upgrading:                                                             
 bash-5.1.8-9.el9.x86_64                            baseos       1.7 MB
  replacing bash-5.1.8-6.el9_1.x86_64                                  
 libacl-2.3.1-4.el9.x86_64                          baseos      22.6 kB
  replacing libacl-2.3.1-3.el9.x86_64                                  
 systemd-libs-252-51.el9_6.3.rocky.0.1.x86_64       baseos     682.9 kB
   replacing systemd-libs-252-18.el9.x86_64                            
Transaction Summary:
 Installing:       17 packages
 Reinstalling:      0 packages
 Upgrading:         3 packages
 Obsoleting:        0 packages
 Removing:          0 packages
 Downgrading:       0 packages
Downloading packages...
Running transaction test...
Updating: bash;5.1.8-9.el9;x86_64;baseos
Updating: systemd-libs;252-51.el9_6.3.rocky.0.1;x86_64;baseos
Updating: libacl;2.3.1-4.el9;x86_64;baseos
Installing: elfutils-libelf;0.192-6.el9_6;x86_64;baseos
Installing: libbpf;2:1.5.0-1.el9;x86_64;baseos
Installing: acl;2.3.1-4.el9;x86_64;baseos
Installing: systemd-rpm-macros;252-51.el9_6.3.rocky.0.1;noarch;baseos
Installing: libmnl;1.0.4-16.el9_4;x86_64;baseos
Installing: psmisc;23.4-3.el9;x86_64;baseos
Installing: libseccomp;2.5.2-2.el9;x86_64;baseos
Installing: kmod-libs;28-10.el9;x86_64;baseos
Installing: expat;2.5.0-5.el9_6;x86_64;baseos
Installing: dbus;1:1.12.20-8.el9;x86_64;baseos
Installing: systemd-pam;252-51.el9_6.3.rocky.0.1;x86_64;baseos
Installing: systemd;252-51.el9_6.3.rocky.0.1;x86_64;baseos
Installing: dbus-common;1:1.12.20-8.el9;noarch;baseos
Created symlink /etc/systemd/system/sockets.target.wants/dbus.socket → /usr/lib/systemd/system/dbus.socket.
Created symlink /etc/systemd/user/sockets.target.wants/dbus.socket → /usr/lib/systemd/user/dbus.socket.
Installing: dbus-broker;28-7.el9;x86_64;baseos
Created symlink /etc/systemd/system/dbus.service → /usr/lib/systemd/system/dbus-broker.service.
Created symlink /etc/systemd/user/dbus.service → /usr/lib/systemd/user/dbus-broker.service.
Installing: net-tools;2.0-0.64.20160912git.el9;x86_64;baseos
Installing: iputils;20210202-11.el9_6.3;x86_64;baseos
Installing: iproute;6.11.0-1.el9;x86_64;baseos
Cleanup: systemd-libs;252-18.el9;x86_64;installed
Cleanup: bash;5.1.8-6.el9_1;x86_64;installed
Cleanup: libacl;2.3.1-3.el9;x86_64;installed
Complete.
--> a49d0e0000a6
[2/2] STEP 5/5: ENTRYPOINT ["/pinger-static"]
[2/2] COMMIT registry.class.syscallx86.com/jdvorak/pinger:0.5.0
--> 78199207bf3a
Successfully tagged registry.class.syscallx86.com/jdvorak/pinger:0.5.0
78199207bf3a5ce6444b313fce123702aff70b894773c1b920b20b117e1192ea
[jdvorak@probee-1 build]$ podman images
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
[jdvorak@probee-1 build]$ su -
Last login: Tue Nov 25 12:48:34 CET 2025 on pts/0
[root@probee-1 ~]# podman images
REPOSITORY                                    TAG         IMAGE ID      CREATED         SIZE
registry.class.syscallx86.com/jdvorak/pinger  0.5.0       78199207bf3a  13 seconds ago  178 MB
docker.io/ustclug/rocky                       9-minimal   1b5159984f02  6 months ago    121 MB
```

### Docker push to remote registry

```bash
$ su -
Last login: Tue Nov 25 12:48:34 CET 2025 on pts/0
[root@probee-1 ~]# podman images
REPOSITORY                                    TAG         IMAGE ID      CREATED         SIZE
registry.class.syscallx86.com/jdvorak/pinger  0.5.0       78199207bf3a  13 seconds ago  178 MB
docker.io/ustclug/rocky                       9-minimal   1b5159984f02  6 months ago    121 MB
[root@probee-1 ~]# dockder images
-bash: dockder: command not found
[root@probee-1 ~]# docker push registry.class.syscallx86.com/jdvorak/pinger:0.5.0
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Getting image source signatures
Copying blob 33cede3e0eb5 done   | 
Copying blob 53ab1c86ad01 done   | 
Copying blob 26f403d47c9b done   | 
Copying blob 49f400400d74 done   | 
Copying config 78199207bf done   | 
Writing manifest to image destination

```


### Run docker contejner

```
# docker run --rm -d --name pinger registry.class.syscallx86.com/jdvorak/pinger:0.5.0
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
21d944e527fd8aa76242b157016bc7ae97ee77fa8d6898497d68f9ff3e395e65
[root@probee-1 ~]# docker ps
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
CONTAINER ID  IMAGE                                               COMMAND     CREATED        STATUS        PORTS       NAMES
21d944e527fd  registry.class.syscallx86.com/jdvorak/pinger:0.5.0              4 seconds ago  Up 4 seconds              pinger
```

### Inspect ip address ans namespaces

```bash
# docker inspect pinger
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
[
     {
          "Id": "2d89aca81484a439bf8f5c08c7bbe145b0a6f5994f21292db9a0420f82b205db",
          "Created": "2025-06-17T14:26:10.49928721+02:00",
          "Path": "/pinger-static",
.
          "Mounts": [],
          "Dependencies": [],
          "NetworkSettings": {
               "EndpointID": "",
               "Gateway": "10.88.0.1",
               "IPAddress": "10.88.0.6",
               "IPPrefixLen": 16,
.
.
```