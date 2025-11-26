# Lab 02 – Docker / Podman

V tomhle labu se zaměříme na práci s Dockerem (resp. Podmanem).  
Pozor: i když budete instalovat balík `docker`, na RHEL-based distribucích se ve skutečnosti nainstaluje **Podman**.  
To je v pořádku – příkazy i parametry jsou totožné a příkaz `docker` je jen symbolický link na `podman`.

Stejně jako v předchozím labu si rozmyslete, které úlohy budete provádět jako root a které jako běžný uživatel.

---

## Úlohy

1. **Nainstalujte Docker** (resp. Podman – RHEL způsob 😊).

2. **Pro aplikaci Pinger**, kterou jste zkompilovali v minulém labu,  
   vytvořte **jednoduchý Dockerfile**.

3. **Vytvořte image** a nahrajte ji do vzdálené registry:  
   `registry.class.syscallx86.com/<username>/pinger:<verze>`  
   - Image by měla obsahovat nástroje jako:
     - `net-tools`, `iproute`, `iputils`, `bash`, `procps-ng`
     - další balíčky dle uvážení
   - Jako base image můžete použít např.:  
     `docker.io/ustclug/rocky:9-minimal`
   - docker registry jsou bez loginu a tls

4. **Spusťte Pinger uvnitř containeru.**

5. **Optional: nakonfiguruj portmapping portu aplikace aby byla viditelná zvenku**
    - docker run -p 8080:8080 <image>

6. Pomocí **`docker inspect <container-id>`** zjistěte:
   - běžící reálné PID procesu na hostiteli  
   - IP adresu containeru

5. **Otestujte aplikaci pomocí `curl`.**

7. Otestujte možnosti **RCA** pomocí `curl`  
   (viz předchozí lab).

8. Zkoušejte různé varianty Dockerfilu, vytvořte si novou verzi image (tag) – např. omezte množství instalovaných balíčků –  
   a porovnejte dopad na:
   - RCA výstup,  
   - běh aplikace,  
   - možnosti troubleshootingu.
   - velikost image (docker image <jmeno>)

9. Pomocí `docker pull` a `docker run` zkuste spustit **image kolegy/kolegyně**.  
   (Pozor na namespace! 😀)

---

## Diskuse

- Porovnejte výsledky Pingeru v containeru vs. na „legacy“ běhu (VM).
- Kolik vrstev má vaše výsledná image?  
  Je možné ji nějak optimalizovat (např. minimal base image, sloučení RUN kroků)?
