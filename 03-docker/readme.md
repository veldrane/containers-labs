## Lab - Docker/Podman

Poslední lab je zaměřen na docker nebo jiný orchestrátor.

### Tasky

- Nainstaluj si docker 
- Pro aplikaci Pinger vytvoř jednoduchy dockerfile
- Vytvoř image a nahraji do remote registry registry.class.syscallx86.com/username/pinger:latest
- Spusť pinger v rámci dockeru.
- Otestuj curlem
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