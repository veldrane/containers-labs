# 01 Lab – Instalace demonstrační aplikace

V téhle první části budeme potřebovat jednoduchou ukázkovou aplikaci, na které si demonstrujeme rozdíly mezi během na *legacy* systému (VM) a v kontejnerech – ať už z pohledu runtimu, nasazení nebo zabezpečení.

Veškerá práce bude probíhat na vašem přiděleném serveru **probee-x**. Jump server slouží jen k přístupu do labu a případně k rychlému otestování aplikace přes `curl`.

Z jump serveru na svůj probee se přihlásíte bez hesla pomocí přiděleného účtu. Stejně tak se bez hesla přihlásíte i na roota na své stanici probee-x. Je na vás, zda budete úlohy provádět jako root, nebo jako svůj běžný uživatel – záleží na situaci a co chcete otestovat.

Lab je koncipovaný tak, abyste si „osahali“ základní workflow buildu libovolné OSS aplikace na Linuxu a jejího nasazení. Typicky to zahrnuje:

- Build/kompilaci přes `make` nebo podobné nástroje (standard v Unix/Linux světě)
- Případné povolení firewall pravidel
- Pochopení rozdílů mezi během pod rootem a pod běžným uživatelem
- Základní linuxový tooling (pokud s ním nemáte zkušenosti), jako např.:
  - zobrazení procesů, logů, environment proměnných
  - instalaci balíčků a pomocných nástrojů
  - základní testování serverových aplikací (`curl`, `jq`, …)
- Uvědomění si rizik běhu aplikace pod právy roota

---

## Úlohy

1. **Naklonujte si repozitář** s demonstrační aplikací:  
   `https://github.com/veldrane/containers-app`  
   (Git je nainstalovaný.)

2. **Dle instrukcí aplikaci zkompilujte a spusťte.**
   - Pozor! Projekt obsahuje standardní GitHubové `README.md`.  
     Není potřeba dělat všechny kroky – stačí aplikaci zkompilovat a pomocí  
     `sudo make install` ji nainstalovat do nějaké spustitelné cesty  
     (např. `/usr/local/bin`).
   - Veškerý potřebný toolchain (Rust, musl, automake) už je nainstalovaný.

3. **Povolte lokální firewall port 8080** (`firewall-cmd`), ideálně i trvale (`--permanent`):  
   https://firewalld.org/documentation/howto/open-a-port-or-service.html

4. **Ověřte funkčnost pomocí `curl`** – jak přímo z vašeho probee-x, tak z jump serveru.

5. Aplikace vrací výstup v JSONu → **použijte `jq`** pro hezčí formátování.  
   (`jq` je nainstalované.)

6. **Vyzkoušejte spuštění aplikace jak pod rootem, tak pod svým uživatelem.**

7. Až budete hotovi, dejte mi vědět – **lab ještě nekončí**. 🙂

---

## Diskuse

- Prozkoumejte výstup aplikace. Je na něm něco zvláštního?