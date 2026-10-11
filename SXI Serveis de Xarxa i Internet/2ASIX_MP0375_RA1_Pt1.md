## Entorn i configuració de xarxa

Partiràs de dues màquines virtuals: una amb Ubuntu Server que farà de servidor DNS i una altra amb Ubuntu Desktop que actuarà com a client. Totes dues han de tenir IP estàtica dins d’una xarxa interna aïllada. La configuració de xarxa, tant del servidor com del client, s’ha de fer amb Netplan. No es farà servir l’entorn gràfic ni NetworkManager per a aquesta part. Hauràs de documentar els fitxers Netplan que modifiquis, les adreces assignades, la màscara, la porta d’enllaç i els DNS configurats. També és recomanable fer una snapshot de les màquines abans de començar a tocar configuracions crítiques.

MV client i servidor:
![[Pasted image 20261011160326.png]]
![[Pasted image 20261011160356.png]]

**Configuració inicial de xarxa:** S'han identificat les interfícies de xarxa del servidor i del client. Les màquines disposen d'una interfície destinada a la xarxa interna del laboratori DNS i una altra destinada a l'administració remota mitjançant SSH.

![[Pasted image 20261011160617.png]]
![[Pasted image 20261011160633.png]]

**Configuració i comprovació de la xarxa interna:** S'han assignat mitjançant Netplan les adreces IP estàtiques `10.0.0.10/24` al servidor i `10.0.0.20/24` al client. S'ha comprovat la connectivitat entre les dues màquines mitjançant `ping`, obtenint un 0% de pèrdua de paquets.
![[Pasted image 20261011161448.png]]
![[Pasted image 20261011161456.png]]
## Servidor DNS primari

Al servidor hi instal·laràs BIND9 i les utilitats associades. A partir d’aquí, hauràs de crear una zona principal marvel.com i dues zones secundàries o subdominis delegats, que seran els dos superherois que et pertoquin segons la taula de l’annex. Per a cada zona caldrà crear els fitxers de zona directa i inversa corresponents, amb els registres necessaris: SOA, NS, A, PTR, CNAME, MX i els que consideris oportuns. És important que els fitxers estiguin comentats i que expliquis què fa cada línia i per què has pres aquelles decisions. La delegació de subdominis no és un simple exercici de sintaxi: ha de quedar clar que entens com es relacionen la zona pare i les zones filles.

**Instal·lació de BIND9:** S'ha instal·lat BIND9 i s'ha comprovat que el servei `named` està actiu i habilitat a l'arrencada. També s'ha verificat que escolta a l'adreça `10.0.0.10` pel port DNS 53 mitjançant UDP i TCP.
![[Pasted image 20261011162106.png]]

**Configuració i seguretat de BIND9:** S'ha restringit l'accés i la recursió DNS a la xarxa local `10.0.0.0/24`, s'han denegat les transferències de zona no autoritzades i s'ha ocultat la versió del servidor. La configuració s'ha validat correctament amb `named-checkconf`.
![[Pasted image 20261011162407.png]]

**Creació de la zona principal:** S'ha creat la zona directa `marvel.com` amb els registres DNS necessaris. S'ha validat la configuració amb `named-checkzone` i s'ha comprovat mitjançant `dig` que els registres A i MX es resolen correctament.
![[Pasted image 20261011162831.png]]

**Creació del subdomini Wolverine:** S'ha creat la zona `wolverine.marvel.com` i s'ha delegat des de la zona pare `marvel.com` mitjançant un registre NS i el corresponent glue record. S'ha validat la zona i comprovat la resolució dels registres A i MX.
![[Pasted image 20261011163135.png]]

**Validació de les zones DNS:** S'han validat correctament la zona pare `marvel.com` i les zones filles `wolverine.marvel.com` i `deadpool.marvel.com`. Els dos subdominis s'han delegat al servidor autoritatiu `ns.marvel.com`.
![[Pasted image 20261011164159.png]]

**Configuració de la zona inversa:** S'ha creat la zona inversa `0.0.10.in-addr.arpa` per a la xarxa `10.0.0.0/24` i s'han configurat registres PTR per relacionar les adreces IP amb els seus noms DNS. La resolució inversa s'ha comprovat correctament mitjançant `dig -x`
![[Pasted image 20261011164448.png]]
## Seguretat bàsica del servei

Com que el DNS és un servei crític, hauràs d’aplicar mesures bàsiques de seguretat. No es tracta de fer una configuració avançada, sinó d’evitar que el teu servidor es converteixi en un open resolver o que qualsevol pugui transferir les teves zones. Hauràs de restringir les consultes i la recursió només a la xarxa local, denegar les transferències de zona no autoritzades i ocultar la versió del servei. També comprovaràs que BIND s’executa amb l’usuari adequat i que AppArmor està actiu. A l’informe hauràs d’explicar què fa cada mesura i per què és important en un entorn real.

**Seguretat del servidor DNS:** S'ha comprovat que les transferències de zona AXFR estan bloquejades per a clients no autoritzats i que la versió real de BIND9 està oculta.
![[Pasted image 20261011165212.png]]

**Usuari del servei i AppArmor:** S'ha comprovat que el procés `named` s'executa amb l'usuari restringit `bind` i no com a root. També s'ha verificat que AppArmor està actiu, afegint una capa de protecció al servidor.
![[Pasted image 20261011165737.png]]
## Gestió, monitorització i client

Utilitzaràs les comandes de systemctl per iniciar, aturar i reiniciar el servei, i comprovaràs que s’inicia automàticament en arrencar el sistema. Activaràs el registre de consultes i monitoritzaràs el servei mentre fas peticions, revisant tant el syslog com el fitxer de log específic de BIND si el configures. Al client, configuraràs la xarxa amb Netplan i faràs que el DNS primari apunti al teu servidor. Hauràs de raonar si cal o no configurar un DNS secundari en aquesta fase i justificar-ho. Finalment, comprovaràs la configuració del client i el contingut del seu resolv.conf.

**Configuració DNS del client:** S'ha configurat mitjançant Netplan el servidor `10.0.0.10` com a DNS del client i el domini de cerca `marvel.com`.
![[Pasted image 20261011164802.png]]

**Proves des del client:** S'ha configurat `10.0.0.10` com a servidor DNS del client. Les proves amb `dig` confirmen la resolució directa i inversa de la zona principal i dels subdominis Wolverine i Deadpool, així com el funcionament dels registres MX.
![[Pasted image 20261011164820.png]]

**Comprovació dels registres CNAME:** S'ha verificat que els àlies `web` apunten correctament als registres `www` de cada zona i que aquests resolen finalment la seva adreça IP.
![[Pasted image 20261011165054.png]]

**Monitorització del servei DNS:** S'ha activat el registre de consultes de BIND9 amb `rndc querylog`. Posteriorment s'han realitzat peticions des del client `10.0.0.20` i s'ha comprovat al registre del servei que el servidor rep i processa correctament les consultes DNS.
![[Pasted image 20261011170225.png]]

## Validació i proves

Abans de donar la pràctica per acabada, validaràs la sintaxi de tots els fitxers amb named-checkconf i named-checkzone. Des del client, faràs consultes amb dig, host i nslookup per comprovar la resolució directa i inversa de la zona principal i dels dos subdominis. També comprovaràs la connectivitat amb ping. A més, hauràs de demostrar que les mesures de seguretat funcionen: que no es revela la versió del servei, que una transferència de zona no autoritzada falla i que el servidor no respon com a open resolver des de fora de la xarxa local. Cada prova ha d’anar acompanyada d’una captura i d’una explicació pròpia.





## Vídeos de validació

Per assegurar que la pràctica és real i no copiada, hauràs d’enregistrar vídeos de comprovació. Els vídeos no poden tenir talls injustificats i han d’incloure àudio on expliquis què fas i per què.

- El primer vídeo servirà per presentar l’entorn i explicar la configuració inicial de xarxa i les zones.
    
- El segon es centrarà en la creació dels fitxers de zona i la validació amb named-checkconf i named-checkzone.
    
- El tercer mostrarà la monitorització i les consultes des del client.

Hauràs d’entregar un informe en PDF amb portada, índex, captures de pantalla comentades, justificació de les configuracions, mesures de seguretat aplicades, resolució dels errors que hagis trobat i conclusions. A més, caldrà adjuntar els fitxers els enllaços o arxius dels vídeos de validació. El document ha de ser clar, ordenat i entenedor; no s’acceptaran treballs incomplets o mal redactats.

  
