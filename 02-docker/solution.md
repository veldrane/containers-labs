### Simple Dockerfile

```Dockerfile
FROM ustclug/rocky:9-minimal

LABEL MANTAINER "-veldrane"

COPY pinger-static /

RUN microdnf install -y net-tools iproute iputils bash \
    && chmod +x /pinger-static

ENTRYPOINT ["/pinger-static"]
```


### Docker install

```bash
# dnf -y install docker
```

### Build docker image

```bash
[root@probeeh opt]# mkdir docker-build
[root@probeeh opt]# cd docker-build/         
[root@probeeh docker-build]# cp ../pinger-rootfs/pinger-static .
[root@probeeh docker-build]# vi Dockerfile
[root@probeeh docker-build]# set -o vi
[root@probeeh docker-build]# docker build -t registry.class.syscallx86.com/jdvorak/pinger:v0.5b -f ./Dockerfile 
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
STEP 1/6: FROM ustclug/rocky:9-minimal
✔ docker.io/ustclug/rocky:9-minimal
Trying to pull docker.io/ustclug/rocky:9-minimal...
Getting image source signatures
Copying blob 0bfe07af826e done   | 
Copying blob 8ec988941d66 done   | 
Copying config 1b5159984f done   | 
Writing manifest to image destination
STEP 2/6: LABEL MANTAINER "-veldrane"
.
.
-> db07cf02c0aa
STEP 5/5: ENTRYPOINT ["/pinger-static"]
COMMIT registry.class.syscallx86.com/jdvorak/pinger:v0.5b
--> 032b548c5d6c
Successfully tagged registry.class.syscallx86.com/jdvorak/pinger:v0.5b
032b548c5d6c601fd1752a31f67d3bbaf5b63f5b62cc19c94e8b8fcab50a8e24
[root@probeeh docker-build]# docker images
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
REPOSITORY                                    TAG         IMAGE ID      CREATED             SIZE
registry.class.syscallx86.com/jdvorak/pinger  v0.5b       032b548c5d6c  6 seconds ago       178 MB
<none>                                        <none>      32f373bb5572  About a minute ago  178 MB
docker.io/ustclug/rocky                       9-minimal   1b5159984f02  3 weeks ago         121 MB
[root@probeeh docker-build]# docker push registry.class.syscallx86.com/jdvorak/pinger
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Error: registry.class.syscallx86.com/jdvorak/pinger: image not known
```

### Docker push to remote registry

```bash
[root@probeeh docker-build]# docker push registry.class.syscallx86.com/jdvorak/pinger:v0.5b
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Getting image source signatures
Copying blob f78589ed5b02 done   | 
Copying blob 53ab1c86ad01 done   | 
Copying blob a63a1b80e0ff done   | 
Copying blob 49f400400d74 done   | 
Copying config 032b548c5d done   | 
Writing manifest to image destination
```


### Run docker contejner

```
# docker run --rm -d --name pinger registry.class.syscallx86.com/jdvorak/pinger:v0.5b
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
2d89aca81484a439bf8f5c08c7bbe145b0a6f5994f21292db9a0420f82b205db
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