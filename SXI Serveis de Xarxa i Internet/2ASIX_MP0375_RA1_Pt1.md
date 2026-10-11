## Entorn i configuració de xarxa

Partiràs de dues màquines virtuals: una amb Ubuntu Server que farà de servidor DNS i una altra amb Ubuntu Desktop que actuarà com a client. Totes dues han de tenir IP estàtica dins d’una xarxa interna aïllada. La configuració de xarxa, tant del servidor com del client, s’ha de fer amb Netplan. No es farà servir l’entorn gràfic ni NetworkManager per a aquesta part. Hauràs de documentar els fitxers Netplan que modifiquis, les adreces assignades, la màscara, la porta d’enllaç i els DNS configurats. També és recomanable fer una snapshot de les màquines abans de començar a tocar configuracions crítiques.

## Servidor DNS primari

Al servidor hi instal·laràs BIND9 i les utilitats associades. A partir d’aquí, hauràs de crear una zona principal marvel.com i dues zones secundàries o subdominis delegats, que seran els dos superherois que et pertoquin segons la taula de l’annex. Per a cada zona caldrà crear els fitxers de zona directa i inversa corresponents, amb els registres necessaris: SOA, NS, A, PTR, CNAME, MX i els que consideris oportuns. És important que els fitxers estiguin comentats i que expliquis què fa cada línia i per què has pres aquelles decisions. La delegació de subdominis no és un simple exercici de sintaxi: ha de quedar clar que entens com es relacionen la zona pare i les zones filles.

## Seguretat bàsica del servei

Com que el DNS és un servei crític, hauràs d’aplicar mesures bàsiques de seguretat. No es tracta de fer una configuració avançada, sinó d’evitar que el teu servidor es converteixi en un open resolver o que qualsevol pugui transferir les teves zones. Hauràs de restringir les consultes i la recursió només a la xarxa local, denegar les transferències de zona no autoritzades i ocultar la versió del servei. També comprovaràs que BIND s’executa amb l’usuari adequat i que AppArmor està actiu. A l’informe hauràs d’explicar què fa cada mesura i per què és important en un entorn real.

## Gestió, monitorització i client

Utilitzaràs les comandes de systemctl per iniciar, aturar i reiniciar el servei, i comprovaràs que s’inicia automàticament en arrencar el sistema. Activaràs el registre de consultes i monitoritzaràs el servei mentre fas peticions, revisant tant el syslog com el fitxer de log específic de BIND si el configures. Al client, configuraràs la xarxa amb Netplan i faràs que el DNS primari apunti al teu servidor. Hauràs de raonar si cal o no configurar un DNS secundari en aquesta fase i justificar-ho. Finalment, comprovaràs la configuració del client i el contingut del seu resolv.conf.

## Validació i proves

Abans de donar la pràctica per acabada, validaràs la sintaxi de tots els fitxers amb named-checkconf i named-checkzone. Des del client, faràs consultes amb dig, host i nslookup per comprovar la resolució directa i inversa de la zona principal i dels dos subdominis. També comprovaràs la connectivitat amb ping. A més, hauràs de demostrar que les mesures de seguretat funcionen: que no es revela la versió del servei, que una transferència de zona no autoritzada falla i que el servidor no respon com a open resolver des de fora de la xarxa local. Cada prova ha d’anar acompanyada d’una captura i d’una explicació pròpia.

## Vídeos de validació

Per assegurar que la pràctica és real i no copiada, hauràs d’enregistrar vídeos de comprovació. Els vídeos no poden tenir talls injustificats i han d’incloure àudio on expliquis què fas i per què.

- El primer vídeo servirà per presentar l’entorn i explicar la configuració inicial de xarxa i les zones.
    
- El segon es centrarà en la creació dels fitxers de zona i la validació amb named-checkconf i named-checkzone.
    
- El tercer mostrarà la monitorització i les consultes des del client.

Hauràs d’entregar un informe en PDF amb portada, índex, captures de pantalla comentades, justificació de les configuracions, mesures de seguretat aplicades, resolució dels errors que hagis trobat i conclusions. A més, caldrà adjuntar els fitxers els enllaços o arxius dels vídeos de validació. El document ha de ser clar, ordenat i entenedor; no s’acceptaran treballs incomplets o mal redactats.

| Ramon Roda | wolverine.marvel.com | deadpool.marvel.com |
| ---------- | -------------------- | ------------------- |
  
