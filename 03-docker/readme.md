## Lab - Docker/Podman

Poslední lab je zaměřen na docker nebo jiný orchestrátor.

### Tasky

- Nainstaluj si docker 
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

- Porovnej deploy v legacy, ve "vlastním kontejneru", pomocí docker buildu.
- Přes docker inspect identifikuj namespacy pod kterým kontejner běží
- Přes netns se zkus do daného ns připojit
- Testuj pinger a porovnej výsledky z legacy, vlastního kontejneru, dockeru.