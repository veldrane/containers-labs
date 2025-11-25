# Lab 03 – Containers obecně (Linux namespaces)

V této části si prohloubíme znalosti o kontejnerech jako takových.  
Seznámíme se s Linux namespaces, ukážeme si jejich vazbu na Docker/Podman kontejnery a podíváme se, jak tyto znalosti využít při troubleshootingu.

---

## Úlohy

1. **Zjistěte PID běžícího Docker kontejneru** pomocí:
   ```
   docker ps
   docker inspect <container-id>
   ```
   Hledejte položku `State.Pid`.

2. Pomocí příkazu:
   ```
   nsenter -t <PID> -n
   ```
   se přepněte do **network namespace** kontejneru.

   Nyní jste v network namespace kontejneru, ale používáte **toolset hosta**.  
   Otestujte:

   - zobrazte konfiguraci IP stacku (`ip a`, `ip r`, `ss -ltn`, …)
   - pro odvážné:  
     nainstalujte si na hosta `tcpdump` (`dnf install tcpdump`)  
     a sledujte síťový provoz kontejneru
   - zjistěte si pomocí `id` aktuálního uživatele

3. Pomocí `exit` ukončete relaci z `nsenter`.

4. Poté se připojte **dovnitř kontejneru**:
   ```
   docker exec -it <container-id> bash
   ```

5. Uvnitř kontejneru vyzkoušejte:

   - vypsat běžící procesy (`ps aux`)
   - spustit `tcpdump` (pokud je v image k dispozici)
   - vypsat IP adresu kontejneru (`ip a`)
   - zjistit aktuálního uživatele (`id`) a porovnat s výsledkem z `nsenter`

6. Vyzkoušejte si přepnutí i do **jiných typů namespaces**, např.:

   - PID namespace  
     ```
     nsenter -t <PID> -p bash
     ```
   - mount namespace  
     ```
     nsenter -t <PID> -m bash
     ```

   Uvnitř porovnejte:  
   - viditelné procesy  
   - viditelné mounty (`mount`, `cat /proc/mounts`)  
   - další rozdíly v prostředí

---

## Diskuse

- Jak Linux namespaces ovlivňují běh aplikace a její komunikaci s okolním OS?
- Jaký je rozdíl mezi připojením přes `nsenter` a `docker exec`?