![[Pasted image 20261005162606.png]]
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p><strong>RA1. Seguretat en dispositius mòbils i IoT</strong></p>
<p><em>Dispositius IoT</em></p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

| <span class="mark">Nom:</span> | Ramon | <span class="mark">Cognoms:</span> | Roda Adame |
| ------------------------------ | ----- | ---------------------------------- | ---------- |

En aquesta segona pràctica, l’alumnat realitzarà l’analisi de
Vulnerabilitats i Seguretat en MQTT i CoAP com a sistemes operatius IoT
instal·lat en una màquina virtual. L’alumnat haurà de realitzar
l’anàlisi dels problemes de seguretat i prendre les mesures oportunes
per tal de minimitzar, corregir, i/o eliminar les vulnerabilitats
trobades.

## <span class="mark">Exercici 1 (50%)</span>

## Anàlisi de Seguretat en el Protocol MQTT

### Configuració del Broker MQTT Insegur (VM 1)

Instal·leu el broker Mosquitto a la VM 1 i configureu-lo per permetre
connexions externes sense autenticació.


1.  Instal·leu el servei Mosquitto:

> \>sudo apt update && sudo apt install -y mosquitto mosquitto-clients

Primer he configurat el servidor amb la 192.168.56.10
![[Pasted image 20261006184448.png]]

i el client amb la 192.168.56.20 
![[Pasted image 20261006184520.png]]

Ara si, he instalat mosquitto al server
![[Pasted image 20261006184829.png]]
	
![[Pasted image 20261006184918.png]]

2.  Per defecte, les versions recents de Mosquitto bloquegen connexions
    > externes no anònimes. Creeu un fitxer de configuració permissiu
    > per al laboratori:

> \>sudo nano /etc/mosquitto/conf.d/lab_insegur.conf
>
> ***listener 1883 0.0.0.0***
>
> ***allow_anonymous true***

![[Pasted image 20261006185251.png]]

3.  Reinicieu el servei:

\>sudo systemctl restart mosquitto

![[Pasted image 20261006185356.png]]

![[Pasted image 20261006185445.png]]

### Captura de Tràfic no Xifrat / Sniffing (VM 2)

L'objectiu és capturar les credencials o dades sensibles enviades a
través de MQTT sense TLS.

1.  A la **VM 2**, instal·leu l'eina de captura de paquets i els clients
    > de Mosquitto:

> \>sudo apt update && sudo apt install -y wireshark tshark
> mosquitto-clients

![[Pasted image 20261006185800.png]]

2.  Obriu Wireshark de forma gràfica, seleccioneu la interfície i
    > apliqueu el filtre **mqtt**.

> Podríeu Iniciar la captura de paquets a la interfície de xarxa
> (substituïu **eth0** per la vostra interfície per exemple **enp0s3**):
>
> \>sudo tshark -i **eth0** -f "tcp port 1883" -Y "mqtt" -V

![[Pasted image 20261006191841.png]]

3.  Des d'una altra terminal a la **VM 2** (o des de la VM 1), simuleu
    > un dispositiu IoT enviant informació sensible (per exemple,
    > credencials de la xarxa Wi-Fi local):

> \>mosquitto_pub -h 192.168.56.10 -t "casa/sensors/wifi_config" -m
> '{"ssid": "XarxaPrivada", "password": "ClauSuperSegura123"}'

![[Pasted image 20261006191754.png]]

4.  Identifiqueu a la captura de Wireshark/Tshark el paquet ***Publish
    > Message*** i cerqueu el camp *Payload* per comprovar que les dades
    > viatgen en text plàning.
![[Pasted image 20261006192059.png]]

![[Pasted image 20261006192129.png]]

![[Pasted image 20261006192254.png]]
### Subscripció Global (Wildcards) i Injecció de Comandes (VM 2)

MQTT permet l'ús de comodins com \# (tots els nivells) i + (un nivell).
Sense control d'accés (ACL), qualsevol pot llegir totes les
comunicacions o enviar comandes no autoritzades.

1.  A la **VM 2** (Atacant), executeu una subscripció global per
    > escoltar tot el tràfic del broker:

> \>mosquitto_sub -h 192.168.56.10 -t "#" -v
![[Pasted image 20261006192538.png]]

2.  A la **VM 1** (Simulant dispositius legítims), publiqueu contingut
    > en diversos canals:

> \>mosquitto_pub -h 192.168.56.10 -t "industria/caldera/temp" -m "75C"
>
> \>mosquitto_pub -h 192.168.56.10 -t "domotica/porta/estat" -m "TANCAT"
![[Pasted image 20261006192754.png]]

3.  Comproveu que la VM 2 rep tot el tràfic.
![[Pasted image 20261006192824.png]]
4.  A la **VM 2**, injecteu una ordre falsa per alterar l'estat d'un actuador crític:  
    > \>mosquitto_pub -h 192.168.56.10 -t "domotica/porta/comanda" -m
    > "OBRIR_PORTA"

### Mitigació: Hardening de MQTT amb Autenticació per Usuari/Contrasenya

1.  A la **VM 1**, creeu un fitxer de contrasenyes amb un usuari
    > anomenat **actuador1**:

> \>sudo mosquitto_passwd -c /etc/mosquitto/passwd_lab actuador1
![[Pasted image 20261006193239.png]]

![[Pasted image 20261006193253.png]]
2.  Introduïu la contrasenya que vulgueu quan la demani.*  
    > *

3.  Modifiqueu el fitxer **/etc/mosquitto/conf.d/lab_insegur.conf:**

> ***listener 1883 0.0.0.0***
>
> ***allow_anonymous false***
>
> ***password_file /etc/mosquitto/passwd_lab***

![[Pasted image 20261008155445.png]]

4.  Assignem la propietat del fitxer de contrasenyes a l'usuari de
    > Mosquitto

> \>sudo chown mosquitto:mosquitto /etc/mosquitto/passwd_lab

5.  Li donem permisos de lectura:

> \>sudo chmod 640 /etc/mosquitto/passwd_lab

6.  Reinicieu el broker:

> \>sudo systemctl restart mosquitto

![[Pasted image 20261008155655.png]]

7.  A la **VM 2**, intenteu publicar sense credencials (ha de fallar amb
    > error **Connection Refused: not authorised**):

> \>mosquitto_pub -h 192.168.56.10 -t "test" -m "hola"
![[Pasted image 20261008160324.png]]
8.  Publiqueu indicant les credencials correctes:

> \>mosquitto_pub -h 192.168.56.10 -t "test" -m "hola" -u "actuador1" -P
> "LA_VOSTRA_CONTRASENYA"
![[Pasted image 20261008160558.png]]

![[Pasted image 20261008160701.png]]
## <span class="mark">Exercici 2 (50%)</span>

## Anàlisi de Seguretat en el Protocol CoAP

CoAP és un protocol lleuger basat en **UDP** dissenyat per a dispositius
restringits. Utilitza mètodes REST (GET, POST, PUT, DELETE) similars a
HTTP.

### Desplegament d'un Servidor CoAP (VM 1)

1.  A la **VM 1**, instal·leu les eines de **libcoap**:

> \>sudo apt update && sudo apt install -y libcoap3-bin

2.  Executeu el servidor CoAP de proves escoltant a la interfície de
    > xarxa:

> \>coap-server-openssl -A 192.168.56.10 -p 5683
![[Pasted image 20261008161417.png]]
### Inspecció de Recursos i Atac de Lectura/Escriptura no Autoritzada (VM 2)

1.  A la **VM 2**, instal·leu el client CoAP:

> \>sudo apt update && sudo apt install -y libcoap3-bin

2.  Descobriu els recursos disponibles al servidor mitjançant el recurs
    > estàndard **.well-known/core**:

> \>coap-client-openssl -m get coap://192.168.56.10/.well-known/core
![[Pasted image 20261008161430.png]]
3.  Obtingueu informació d'un recurs (ex. l'hora del servidor):

> \>coap-client-openssl -m get coap://192.168.56.10/time

![[Pasted image 20261008161535.png]]

4.  En un altre terminal de la **VM 2**, inicieu una captura amb
    > Wireshark/Tshark filtrant per UDP port 5683:

> \>sudo tshark -i eth0 -f "udp port 5683" -Y "coap" -V
![[Pasted image 20261008161900.png]]
5.  Realitzeu una petició **PUT** per modificar o crear un recurs remot:

> \>coap-client-openssl -m put -e "NOVA_CONFIGURACIO_MALICIOSA"
> coap://192.168.56.10/example_data

6.  Verifiqueu a la captura com tota la capçalera CoAP i el payload UDP
    > es transmeten clarament sense cap mena de xifratge.

### Mitigació en CoAP: DTLS (Datagram Transport Layer Security)

Mentre que HTTP utilitza TLS (**https://**), CoAP utilitza **DTLS**
(**coaps://**, port per defecte **5684**).

1.  Per implementar seguretat a CoAP, cal configurar un mecanisme de
    > claus compartides (**PSK** - *Pre-Shared Key*) o certificats
    > X.509.

2.  Executeu el servidor CoAP utilitzant DTLS amb una clau precompartida
    > (PSK):

> \>coap-server-openssl -A 192.168.56.10 -p 5684 -k
> "LaMevaClauSecreta123" -u "usuariIoT"

3.  Intenteu accedir des de la **VM 2** utilitzant el protocol no xifrat
    > (Hauria de respondre, tot i que intentem accedir des del client
    > sense validar-nos. Això és degut a que al servidor hem obert el
    > servei segur i no segur al port 5684 ):

> \>coap-client-openssl -m get coap://192.168.56.10:5684/time

4.  Accediu utilitzant el protocol **coaps://** i les credencials PSK
    > establertes:

> \>coap-client-openssl -m get -k "LaMevaClauSecreta123" -u "usuariIoT"
> coaps://192.168.56.10:5684/time
>
> Anem ara al servidor, a restringir l’accés en mode segur. (Ara no
> especifiquem el port, per tant serà el port 5684 en mode segur)

5.  Executeu el servidor CoAP utilitzant DTLS amb una clau precompartida
    > (PSK):

> \>coap-server-openssl -A 192.168.56.10 -k "LaMevaClauSecreta123" -u
> "usuariIoT"

6.  Intenteu accedir des de la **VM 2** utilitzant el protocol no xifrat
    > (Ha de fallar. Això és degut a que al servidor hem obert el servei
    > segur al port 5684 ):

> \>coap-client-openssl -m get coap://192.168.56.10:5684/time

7.  Accediu utilitzant el protocol **coaps://** i les credencials PSK
    > establertes:

> \>coap-client-openssl -m get -k "LaMevaClauSecreta123" -u "usuariIoT"
> coaps://192.168.56.10:5684/time

## Simulació d'Atac d'Amplificació UDP i IP Spoofing en CoAP(Denegació de Servei - DoS)

El protocol **CoAP** s'executa habitualment sobre UDP (un protocol no
orientat a connexió i sense estat). Aquesta característica el fa
vulnerable a tàctiques de suplantació d'identitat (**IP Spoofing**) i
**Amplificació DoS** (Denegació de Servei).

- **IP Spoofing**: L'atacant modifica la capçalera IP de la petició per
  > fer-se passar per una altra màquina (la víctima).

- **Amplificació DoS**: Una petició molt petita (30–40 bytes) pot
  > generar una resposta molt més extensa (200–500 bytes en demanar
  > recursos com /.well-known/core), multiplicant el tràfic rebut per la
  > víctima per un factor de x5 a x10.

<!-- -->

- A la **VM 2**, instal·leu **nmap** per fer ús de l'eina **nping**:

> \>sudo apt update && sudo apt install -y nmap

- Enviament de la Petició Suplantada (IP Spoofing)

> (*Executeu aquest pas exclusivament en l'entorn de laboratori
> controlat)*
>
> Enviar un paquet UDP amb IP d'origen suplantada (*IP Spoofing*)
> utilitzant **nping:**
>
> \>sudo nping --udp -g 12345 -p 5683 --source-ip 192.168.56.99 -c 1
> 192.168.56.10 **Important**: (useu l’opcció o paràmetre –data de la
> nota tècnica)
>
> *(On **192.168.56.99** seria la IP de la víctima que rebria la
> resposta sense haver-la demanat).*

***Paràmetres utilitzats:***

- **-g 12345**: Port d'origen definit manualment.

- **-p 5683**: Port de destí del servidor CoAP.

- **--source-ip 192.168.56.99**: Adreça IP suplantada (la víctima que
  > rebrà la resposta no sol·licitada).

- **-c 1**: Nombre de paquets a enviar.

> **Nota tècnica:** Perquè el servidor CoAP retorni el recurs de
> directori complet i s'executi l'amplificació real, es pot afegir el
> payload hexadecimal d'una petició GET /.well-known/core mitjançant el
> paràmetre --data "40010001bb2e77656c6c2d6b6e6f776e04636f7265"

- **bb**: Defineix una opció *Uri-Path* d'11 bytes de llargada (0x0B).

- **2e**: El caràcter **.**

- **77656c6c2d6b6e6f776e**: El text **well-known** (10 bytes).

- **04636f7265**: El text **core** (4 bytes).

<!-- -->

- Verifiqueu amb Wireshark, tshark o tcpdump a la IP de la víctima
  > (192.168.56.99) si s'ha rebut la resposta del servidor CoAP.

- Vegeu la diferència de mida entre una petició de pocs bytes i la
  > resposta generada pel servei CoAP observant el contingut a la
  > pantalla o via **tshark**:

  - Mida sol·licitud CoAP GET: **~30-40 bytes  
    > **

  - Mida resposta CoAP (**.well-known/core**): Pot superar fàcilment els
    > **200-500 bytes** (factor d'amplificació de x5 a x10).

## 

## <span class="mark">Exercici 3 (Opcional)</span>

Afegeix les línies de configuració a Mosquitto per activar l'ús de
certificats digitals (TLS) al port 8883. Realitza la configuració al
servidor i al client, i mostra el correcte funcionament amb certificats.
