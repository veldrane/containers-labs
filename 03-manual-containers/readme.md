## Lab - Vytváříme kontejner bez orchestrátoru

V rámci tohoto labu si vytvoříme kontejner ručně, bez dockeru/podmanu etc. Měli byste dojít k poznání co to vlastně kontejner 
je a co se děle tzv. "pod pokličkou". Zaměříme se na základní stavební kameny, vytvoříme si rootfs, virtualní network
zařízení a izolujeme v tomto prostředí naší aplikaci kterou jsme si vydeplojovali v předchozím labu. V solutions.md je 
step by step celé řešení, nicméně doporučuji se tím zkusit protrápit sám/s pomocí chatgpt apod :). Pro většinu kroků 


### Tasky

- Vytvoř si separátní rootfs v /opt/rootfs-pinger pomocí dnf a parametru installroot 
    - V rámci deploymentu nezapomeň na balíčky: passwd shadow-utils bash net-tools iproute curl vim-minimal nano iputils
- Do nového rootfs přenes app pinger (nejlépe statickou verzi)
- Zkompiluj pinger jako statickou binárku,vysledný soubor nahrej do /opt/rootfs-pinger, přidej práva pro spuštění
- Vytvoř network namespace pinger
- Vytvoř zařízení veth-pair, kde jeden konec bude v namespacu pinger a druhý zůstane v hlavním ns
- Zkonfiguruj veth zarizeni v hlavnim ns tak aby melo ip adresu 10.200.x.1/24
- Zkonfiguruj ip pro zarizeni v ns pinger s ip 10.200.x.10/24
- Oba konce veth pair dej do stavu up a zkus ping z hlavniho ns na adresu 10.200.x.10
- Pomocí příkazu ip netns exec pinger unshare vytvoř izolované prostředí
- V novém ns namountuj procfs
- V rámci izolovaného prostředí něj pusť pinger
- Otestuj pinger curlem z hlavního ns
- Optional(viz lab2): Vezmi rootfs co jsi vytvořil a:
    - udělej z něj tgz
    - přes docker load nahraj do lokálního repositáře
    - image otaguj jako registry.class.syscallx86.com/username/pinger-base:latest a pushni do remote registry
    - pust pinger v ramci dockeru

### Diskuse

- Co potřebujeme pro to, aby byl pinger v rámci izolovaného prostředí dostupný i ze serveru jump ?
- Jaké jsou možné scénaře publikování aplikace v takovém prostředí ? 
- Jaký dopad může mít omezení instalovaných balíků do nového rootfs ?
- Je možné pustit aplikaci bez rootfs ? Pokud ano za jakých podmínek ?
- Porovnej deploy v legacy, ve "vlastním kontejneru", pomocí docker buildu.
- Přes docker inspect identifikuj namespacy pod kterým kontejner běží (optional viz Lab2)
- Přes netns se zkus do daného ns připojit (optional viz Lab2)