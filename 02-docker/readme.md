## Lab - Docker/Podman

Druhý lab je zaměřen na docker nebo jiný orchestrátor. Pozor, i když v labu instalujete docker nainstaluje se podman (rhel distro based quirks :)!
Niméně příkazy i paramtetry jsou totožné, lze použít příkaz "docker" (je to symbolický link na podman). Rozmysli si, které tasky budeš dělat pod rootem
a které pod klasickým uživatelem

### Tasky

- Nainstaluj si docker (i když budeš instalovat docker nainstaluje se podman - rhel way :)
- Pro aplikaci Pinger vytvoř jednoduchý Dockerfile.
- Vytvoř image a nahraji do remote registry registry.class.syscallx86.com/username/pinger:latest
    - Image by měla obsahovat net-tools iproute iputils bash proc-ps další balíčkům se meze nekladou
    - pro base image můžeš použít třeba docker.io/ustclug/rocky:9-minimal
- Spusť pinger v rámci dockeru.
- Otestuj curlem
- Přes docker inspect container id zjisti:
    - bežící reálný pid v rámci host serveru
    - ip adresu
- Pomocí přikazu nsenter -n -t PID se přepni do network namespacu (o namespacech budeme mluvit v další kapitole)
    - nyní bys měl být v kontextu network ns pingeru ale máš k dispozici toolset hosta otestuj:
        - zjisti konfiugraci ip stacku
        - pro odvážné: na host si nainstaluj tcpdump a sleduj síťový tok na docker kontejner
- Pomocí příkazu lsns zkus identifikovat namespaces aplikace
- Přes přes příkaz docker exec se připoj do běžícího kontejneru
- Co zkus si vypsat procesy
- Zkus si přes docker pull a start si pustit image kolegy/kolegyně :)


### Diskuse

- Testuj pinger a porovnej výsledky z legacy
- Kolik vrstev má Tvá výsledná image ? Je možná nějaká optimalizace ?
