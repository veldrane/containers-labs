## Lab - Instalace demonstrační aplikace

Pro naše potřeby budeme potřebovat aplikaci na které si demonstrujeme rozdíly mezi během v legacy systému(vm) a kontejnerech
ať již z hlediska samotného runtimu, nasazení nebo zabezpečení.

### Úlohy


- Naklonuj si repositář s demonstrační aplikací z github.com/veldrane/containers-app
- Dle instrukcí aplikaci zkompiluj a spusť
    - Pozor! Projekt obsahuje standartni readme.md pro jakykoliv projekt na githubu
      Neni potřeba dělat všechny kroky. Důležité je jen zkompilovat a přes make install
      nainstalovat applikaci do nějaké spustitelné cesty (/usr/local/bin/ třeba)
- Nezapomeň povolit lokální fw (příkaz firewalld-cmd port 8080)
- Oveř si curlem jak ze samotné probee tak z jump serveru, že aplikace funguje.
- aplikace má výstup v jsonu, použij jq příkaz pro lepší formátování. Jq by mělo být nainstalováno
- Zkus si app pustit jak pod rootem tak pod Tvým lokálním účtem
- Dej mi vědět až budeš hotov, Lab tímto ještě nekončí :)


