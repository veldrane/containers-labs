# Bonus Lab – Vytváříme kontejner ručně (bez orchestrátoru)

V tomto bonusovém labu si ručně vytvoříte vlastní „kontejner“ – tedy izolované prostředí bez použití Dockeru, Podmanu či jiného runtime.  
Cílem je pochopit, **co se pod kapotou skutečně děje**, a z jakých stavebních bloků jsou kontejnery složené.

Budeme pracovat s:

- vlastním **rootfs**,
- Linux **namespaces**,
- **veth** párem,
- izolovaným procesním prostředím,
- a nakonec do tohoto prostředí spustíme aplikaci *pinger*, kterou jste připravovali v předchozích labech.

V `solution.md` je kompletní step-by-step řešení, ale velmi doporučuji se tím nejdřív **protrápit sám** (nebo za asistence ChatGPT 😊).

---

## Úlohy

1. **Vytvořte samostatný rootfs** v `/opt/rootfs-pinger` pomocí:
   ```
   dnf install --installroot=/opt/rootfs-pinger <balíčky>
   ```
   Nezapomeňte na balíčky:
   - `passwd`
   - `shadow-utils`
   - `bash`
   - `net-tools`
   - `iproute`
   - `curl`
   - `vim-minimal` nebo `nano`
   - `iputils`

2. **Zkompilujte pinger jako statickou binárku**  
   → výsledný soubor nahrajte do `/opt/rootfs-pinger/usr/local/bin/`  
   → a přidejte mu spustitelná práva.

3. **Do nového rootfs přeneste statickou binárku pingeru**  
   (pokud jste ji už nezkompilovali přímo pro tento účel).

4. **Vytvořte network namespace**:
   ```
   ip netns add pinger
   ```

5. **Vytvořte veth pair** – jedno rozhraní bude uvnitř `pinger`, druhé v host ns:
   ```
   ip link add veth-host type veth peer name veth-pinger
   ip link set veth-pinger netns pinger
   ```

6. **Nakonfigurujte IP adresu v host namespace**:
   ```
   ip addr add 10.200.x.1/24 dev veth-host
   ```

7. **Nakonfigurujte IP adresu v `pinger` namespace**:
   ```
   ip netns exec pinger ip addr add 10.200.x.10/24 dev veth-pinger
   ```

8. **Uveďte obě strany veth pair do stavu UP**:
   ```
   ip link set veth-host up
   ip netns exec pinger ip link set veth-pinger up
   ```

9. **Otestujte konektivitu** – z hostu pingněte kontejner:
   ```
   ping 10.200.x.10
   ```

10. **Vytvořte izolované prostředí pomocí**:
    ```
    ip netns exec pinger unshare -pfmu --mount-proc bash
    ```

11. **V rámci nového prostředí namountujte procfs**, pokud jej `unshare` neudělal automaticky:
    ```
    mount -t proc proc /proc
    ```

12. **Spusťte pinger uvnitř izolovaného prostředí**:
    ```
    /usr/local/bin/pinger
    ```

13. **Otestujte pinger z hlavního host namespace** pomocí `curl`.

14. **Volitelné (viz Lab 2)** – vytvořený rootfs použijte jako základ Docker image:
    - vytvořte archiv:
      ```
      tar -C /opt/rootfs-pinger -czf pinger-rootfs.tgz .
      ```
    - načtěte jej jako Docker image:
      ```
      docker import pinger-rootfs.tgz pinger-base
      ```
    - otagujte a pushněte:
      ```
      docker tag pinger-base:latest registry.class.syscallx86.com/<username>/pinger-base:latest
      docker push registry.class.syscallx86.com/<username>/pinger-base:latest
      ```
    - spusťte pinger v Dockeru.

---

## Diskuse

- Co je potřeba udělat, aby byl pinger v rámci izolovaného prostředí dostupný i ze serveru *jump*?
- Jaké jsou možné scénáře publikování aplikace v takto vytvořeném prostředí?
- Jaký dopad může mít omezení počtu balíčků ve vlastním rootfs?
- Je možné pustit aplikaci **zcela bez rootfs**? Pokud ano – za jakých podmínek?
- Porovnejte deployment:
  - v legacy systému (VM),
  - ve vámi vytvořeném „ručním kontejneru“,
  - pomocí Docker buildu.
- Pomocí `docker inspect` identifikujte namespaces, ve kterých běží Docker kontejner (optional, viz Lab 2).
- Pomocí `ip netns exec` nebo `nsenter` se zkuste připojit přímo do jeho network namespace (optional, viz Lab 2).