## Lab - Docker/Podman

Druhý lab je zaměřen na docker nebo jiný orchestrátor. Pozor, i když v labu instalujete docker nainstaluje se podman (rhel distro based quirks :)!
Niméně příkazy i paramtetry jsou totožné, lze použít příkaz "docker" (je to symbolický link na podman)

### Tasky

- Nainstaluj si docker (i kdyz budes instalovat docker nainstluje se podman - rhel way - no stress :)
- Pro aplikaci Pinger vytvoř jednoduchý Dockerfile.
- Vytvoř image a nahraji do remote registry registry.class.syscallx86.com/username/pinger:latest
- Spusť pinger v rámci dockeru.
- Otestuj curlem
- Přes docker inspect container id zjisti:
    - bežící reálný pid v rámci host serveru
    - ip adresu
- Pomocí přikazu nsenter -n -t PID se přepni do network namespacu
    - nyní bys měl být v kontextu network ns pingeru ale máš k dispozici toolset hosta otestuj:
        - zjisti konfiugraci ip stacku
        - pro odvážné: na host si nainstaluj tcpdump a sleduj síťový tok na docker kontejner
- Pomocí příkazu lsns zkus identifikovat namespaces aplikace
- Optional: Vezmi rootfs co jsi vytvořil v předchozím labu a:
    - udělej z něj tgz
    - přes docker load nahraj do lokálního repositáře
    - image otaguj jako registry.class.syscallx86.com/username/pinger-base:latest a pushni do remote registry
    - pust pinger v ramci dockeru


### Diskuse

- Testuj pinger a porovnej výsledky z legacy, vlastního kontejneru, dockeru.