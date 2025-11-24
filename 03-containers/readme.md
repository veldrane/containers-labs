## 03 Lab Containers obecně

V této části si prohloubíme znalosti o kontejnerech jako takových. Seznamíme se s linux namespaces,
ukážeme si jejich vazbu na docker/podman kontejner, ukážeme si jak tyto znalosti využít pro případný 
troubleshooting.

### Tasky

- Zjisti PID běžícího docker kontejneru pomocí příkazu docker inspect.
- Pomocí přikazu nsenter -n -t PID se přepni do network namespacu
    - nyní bys měl být v kontextu network ns kontejneru ale máš k dispozici toolset hosta otestuj:
        - zjisti konfiugraci ip stacku
        - pro odvážné: na host si nainstaluj tcpdump a sleduj síťový tok na docker kontejner
            - pro instalaci použij příkaz dnf nebo yum
        - zjisti si přes přikaz id aktuálníhu uživatele
- Přes příkaz exit ukonči relaci uvnitř nsenter.
- Přes přes příkaz docker exec se připoj do běžícího kontejneru
- Zkus si 
    - vypsat procesy
    - pustit tcpdump uvnitř kontejneru
    - přes příkaz ip vypsat ip adresu uvnitr kontejneru.
    - přes příkaz id zjistit aktuálního uživatele a porovnej s pouštěním pře snsenter 
- vyzkoušej si přepnutí do jiného typu namespacu (process, mount apod ) a testuj co v rámci něj vidíš (procesy, mounty apod)

### Diskuse

- Jak namespaces ovlivňují běh aplikace a možnost komunikace s okolním OS světem ?
