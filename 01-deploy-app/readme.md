## Lab - Instalace demonstrační aplikace

Pro naše potřeby budeme potřebovat aplikaci na které si demonstrujeme rozdíly mezi během v legacy systému(vm) a kontejnerech
ať již z hlediska samotného runtimu, nasazení nebo zabezpečení. Veškerá práce by měla probíhat na Vašem přiděleném serveru
probee-x, jump server je pouze pro přístup dol labu, je možné ho také použít pro ověření aplikace přes curl.
Z jump serveru na probee se dostanete bez hesla přes svůj přidělený account, stejně tak bez hesla se přihlásíte na 
roota. Je na Vás zda úlohy budete pravádět pod rootem nebo pod Vaším uživatelem (bude záležet na situaci)

### Úlohy


- Naklonuj si repositář s demonstrační aplikací z github.com/veldrane/containers-app (git je nainstalován)
- Dle instrukcí aplikaci zkompiluj a spusť
    - Pozor! Projekt obsahuje standartni readme.md pro jakykoliv projekt na githubu
      Neni potřeba dělat všechny kroky. Důležité je jen zkompilovat a přes "sudo make install"
      nainstalovat applikaci do nějaké spustitelné cesty (/usr/local/bin/ třeba)
    - Veškerý pot5ebn7 toolkit (rust, musl, automake) je nainstalován
- Nezapomeň povolit lokální fw port 8080 (příkaz firewalld-cmd)
- Oveř si curlem jak ze samotné probee tak z jump serveru, že aplikace funguje.
- aplikace má výstup v jsonu, použij jq příkaz pro lepší formátování. Jq by mělo být nainstalováno
- Zkus si app pustit jak pod rootem tak pod Tvým lokálním účtem
- Dej mi vědět až budeš hotov, Lab tímto ještě nekončí :)

### Diskuse

- Prozkoumej výstup aplikace. Je na ní něco zvláštního ?


