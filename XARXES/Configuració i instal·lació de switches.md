![[Pasted image 20260919125043.png]]
## Què és un switch?

Un **switch** connecta dispositius dins d’una xarxa local (LAN).

```
                 ROUTER
                    │
                    │
                 SWITCH
          ┌─────────┼─────────┐
          │         │         │
         PC       Impressora  AP
```

A diferència del router, que comunica **xarxes diferents**, un switch tradicional de capa 2 s’encarrega principalment de comunicar dispositius dins de la LAN utilitzant les seves **adreces MAC**.

---

# 3.2. Switch gestionable i no gestionable

### Switch no gestionable

És pràcticament _plug and play_:

```
Connectar alimentació
        ↓
Connectar cables
        ↓
Funcionament
```

No acostuma a permetre configuracions avançades.

### Switch gestionable

Permet configurar, entre altres coses:

```
VLAN
Trunks
STP
LACP / EtherChannel
Port Security
PoE
SSH
SNMP
QoS
```

És el tipus que més ens interessa en una xarxa empresarial.

---

# 3.3. Instal·lació física

Abans de començar:

- Comprovar alimentació.
- Comprovar cables Ethernet.
- Comprovar LEDs.
- Identificar ports.
- Identificar uplinks.
- Comprovar si disposa de PoE.
- Comprovar connexió amb router/firewall.
- Comprovar connexions amb altres switches.
- Etiquetar els cables si correspon.

Exemple:

```
ROUTER
  │
  │
Gi0/1
  │
SWITCH
├── Gi0/2 → PC Administració
├── Gi0/3 → PC Administració
├── Gi0/4 → Impressora
├── Gi0/5 → AP
└── Gi0/6 → Segon switch
```

És molt recomanable **documentar què hi ha connectat a cada port**.

---

# 3.4. Accés al switch

Podem administrar un switch gestionable mitjançant:

```
Consola
SSH
Interfície web
```

En una configuració inicial és habitual utilitzar consola:

```
PORTÀTIL
   │
USB / consola
   │
   ▼
 SWITCH
```

Posteriorment podem administrar-lo mitjançant SSH.

---

# 3.5. Configuració inicial Cisco IOS

Entrar al mode privilegiat:

```
Switch> enable
Switch#
```

Entrar al mode de configuració:

```
Switch# configure terminal
Switch(config)#
```

Canviar hostname:

```
Switch(config)# hostname SW-OFICINA-01
```

Ara:

```
SW-OFICINA-01(config)#
```

---

# 3.6. Consultar els ports

Una de les primeres comandes que hauríem de conèixer:

```
show interfaces status
```

Ens permet veure ràpidament:

```
Port
Estat
VLAN
Duplex
Velocitat
Tipus
```

També:

```
show ip interface brief
```

I per consultar una interfície concreta:

```
show interfaces gigabitEthernet 0/5
```

---

# 3.7. Activar i desactivar un port

Entrar al port:

```
configure terminal

interface gigabitEthernet 0/5
```

Desactivar:

```
shutdown
```

Activar:

```
no shutdown
```

Per tant:

```
SW(config)# interface gi0/5
SW(config-if)# shutdown
```

o:

```
SW(config-if)# no shutdown
```

---

# 3.8. Afegir descripcions als ports

És una pràctica molt recomanable.

Per exemple:

```
interface gi0/5
 description PC-RECEPCIO
```

Un AP:

```
interface gi0/10
 description AP-PLANTA-1
```

Uplink:

```
interface gi0/24
 description UPLINK-ROUTER
```

Després podem consultar-ho amb:

```
show interfaces description
```

Això et pot estalviar molt temps en una incidència.

---

# 3.9. Taula MAC

El switch aprèn quines adreces MAC hi ha darrere de cada port.

```
PC
MAC AA:AA:AA...
 │
Gi0/2
 │
SWITCH
```

El switch guarda una associació similar a:

```
MAC                  PORT
AA:AA:AA:AA:AA:AA → Gi0/2
BB:BB:BB:BB:BB:BB → Gi0/3
CC:CC:CC:CC:CC:CC → Gi0/10
```

Consultar-la:

```
show mac address-table
```

Buscar un port:

```
show mac address-table interface gi0/2
```

Aquesta comanda és **molt útil** per saber quin dispositiu està connectat a un port.

---

# 3.10. VLAN

Una **VLAN** permet separar lògicament una xarxa encara que els dispositius estiguin connectats al mateix switch físic.

Per exemple:

```
                SWITCH
       ┌──────────┼──────────┐
       │          │          │
    VLAN 10    VLAN 20    VLAN 30
     ADMIN      EMPLEATS    CONVIDATS
```

Podríem definir:

|VLAN|Nom|Xarxa|
|---|---|---|
|10|ADMIN|192.168.10.0/24|
|20|EMPLEATS|192.168.20.0/24|
|30|CONVIDATS|192.168.30.0/24|

Això crea **dominis de broadcast diferents**.

---

# 3.11. Crear VLAN

VLAN 10:

```
configure terminal

vlan 10
 name ADMIN
exit
```

VLAN 20:

```
vlan 20
 name EMPLEATS
exit
```

VLAN 30:

```
vlan 30
 name CONVIDATS
exit
```

Comprovar:

```
show vlan brief
```

Podríem veure:

```
VLAN   Name
1      default
10     ADMIN
20     EMPLEATS
30     CONVIDATS
```

---

# 3.12. Port ACCESS

Un **port access** pertany normalment a una única VLAN i s’utilitza habitualment per connectar dispositius finals.

Per exemple:

```
PC Administració
      │
      │ VLAN 10
      ▼
    Gi0/2
      │
    SWITCH
```

Configuració:

```
interface gi0/2

switchport mode access

switchport access vlan 10

no shutdown
```

Ara el dispositiu connectat a `Gi0/2` pertany a la VLAN 10.

---

# 3.13. Configurar diversos ports alhora

Si tenim:

```
Gi0/1 → Gi0/8 = VLAN 10
```

podem utilitzar:

```
interface range gi0/1 - 8
```

Després:

```
switchport mode access
switchport access vlan 10
```

Per exemple:

```
interface range gi0/1 - 8
 description PCs-ADMIN
 switchport mode access
 switchport access vlan 10
 no shutdown
```

Això és molt més ràpid que configurar-los un per un.

---

# 3.14. Port TRUNK

Un trunk permet transportar **múltiples VLAN pel mateix enllaç**.

Exemple:

```
SWITCH 1
   │
   │ VLAN 10
   │ VLAN 20
   │ VLAN 30
   │
  TRUNK
   │
   ▼
SWITCH 2
```

Normalment utilitza etiquetatge **IEEE 802.1Q**.

Configuració típica:

```
interface gi0/24

switchport mode trunk
```

Podem limitar les VLAN permeses:

```
switchport trunk allowed vlan 10,20,30
```

---

# 3.15. ACCESS vs TRUNK

Aquesta diferència l’has de tenir molt clara:

|ACCESS|TRUNK|
|---|---|
|Normalment una VLAN|Múltiples VLAN|
|PCs|Switch ↔ Switch|
|Impressores|Switch ↔ Router|
|Alguns dispositius finals|Switch ↔ AP amb múltiples SSID/VLAN|
|Tràfic habitualment sense etiqueta al dispositiu final|Tràfic VLAN etiquetat amb 802.1Q|

Visualment:

```
PC
 │
ACCESS VLAN 10
 │
SWITCH
 │
TRUNK VLAN 10,20,30
 │
SWITCH
 │
ACCESS VLAN 20
 │
PC
```

---

# 3.16. Native VLAN

En un trunk 802.1Q existeix el concepte de **native VLAN**.

Configuració:

```
interface gi0/24

switchport mode trunk

switchport trunk native vlan 99
```

I VLAN permeses:

```
switchport trunk allowed vlan 10,20,30,99
```

En un disseny professional, la VLAN nativa s’ha de configurar de manera coherent als dos extrems del trunk i seguint la política de l’empresa.

---

# 3.17. Comprovar trunks

Una comanda fonamental:

```
show interfaces trunk
```

Ens permet comprovar:

```
Ports trunk
Native VLAN
VLAN permeses
VLAN actives
```

Si dues VLAN funcionen dintre d’un switch però no arriben a l’altre switch, **el trunk és un dels primers llocs que hauríem de revisar**.

---

# 3.18. IP de gestió del switch

Un switch gestionable necessita una IP si volem administrar-lo remotament.

En un switch Cisco de capa 2 podem configurar una SVI, per exemple:

```
interface vlan 99

ip address 192.168.99.2 255.255.255.0

no shutdown
```

Podríem utilitzar:

```
VLAN 99 → GESTIÓ

Router:   192.168.99.1
Switch 1: 192.168.99.2
Switch 2: 192.168.99.3
Switch 3: 192.168.99.4
```

En un switch de capa 2 també podem configurar el gateway de gestió:

```
ip default-gateway 192.168.99.1
```

---

# 3.19. Configurar SSH

És preferible administrar remotament el switch mitjançant protocols segurs com **SSH** en lloc de Telnet.

Exemple Cisco IOS:

```
hostname SW1

ip domain-name empresa.local
```

Crear usuari:

```
username admin privilege 15 secret CONTRASENYA
```

Generar claus RSA:

```
crypto key generate rsa
```

El dispositiu pot demanar la mida de les claus. Cal utilitzar una mida compatible amb l’equip i amb la política de seguretat de l’organització.

Forçar SSH v2 quan el dispositiu ho admeti:

```
ip ssh version 2
```

Configurar línies VTY:

```
line vty 0 15

login local

transport input ssh
```

Des d’un ordinador:

```
ssh admin@192.168.99.2
```

---

# 3.20. Port Security

En switches Cisco compatibles, **Port Security** permet limitar quines o quantes MAC poden utilitzar determinats ports d’accés.

Exemple:

```
interface gi0/2

switchport mode access

switchport port-security
```

Limitar el nombre de MAC:

```
switchport port-security maximum 1
```

Aprenentatge sticky:

```
switchport port-security mac-address sticky
```

Configurar el comportament davant d’una violació:

```
switchport port-security violation restrict
```

Altres modes depenen de la plataforma/configuració.

Consultar:

```
show port-security
```

Port concret:

```
show port-security interface gi0/2
```

---

# 3.21. PoE

**Power over Ethernet (PoE)** permet transportar alimentació elèctrica i dades pel mateix cable Ethernet.

És habitual amb:

```
Punts d'accés Wi-Fi
Telèfons IP
Càmeres IP
```

Exemple:

```
             SWITCH PoE
                 │
       Ethernet + alimentació
                 │
                 ▼
                 AP
```

Per tant, l’AP pot no necessitar una font d’alimentació independent.

En molts switches Cisco podem consultar:

```
show power inline
```

Ens permet veure informació relacionada amb:

```
Port
Dispositiu
Potència
Estat PoE
```

---

# 3.22. STP – Spanning Tree Protocol

STP evita **bucles de capa 2**.

Imaginem:

```
       SW1
      /   \
     /     \
   SW2─────SW3
```

Tenim redundància, però sense mecanismes de prevenció de bucles podríem provocar problemes greus de xarxa.

STP crea una topologia lògica sense bucles bloquejant determinats camins redundants quan és necessari.

Consultar:

```
show spanning-tree
```

Podrem trobar conceptes com:

```
Root Bridge
Root Port
Designated Port
Alternate Port
```

En entorns reals **no s’ha de desactivar STP sense una raó de disseny molt concreta**.

---

# 3.23. PortFast

En ports d’accés connectats a dispositius finals, segons el disseny de xarxa, podem utilitzar PortFast:

```
interface gi0/2

spanning-tree portfast
```

Això permet que el port d’usuari passi més ràpidament a estat de reenviament.

S’utilitza habitualment en ports cap a dispositius finals, **no indiscriminadament en enllaços entre switches**.

---

# 3.24. BPDU Guard

Pot complementar PortFast:

```
interface gi0/2

spanning-tree bpduguard enable
```

Si el port rep BPDUs inesperades, BPDU Guard pot posar-lo en estat de protecció/error segons la plataforma.

Això ajuda a protegir la topologia STP davant connexions inadequades.

---

# 3.25. EtherChannel

EtherChannel permet agrupar diversos enllaços físics en un enllaç lògic.

```
       SWITCH 1
       │ │ │ │
       │ │ │ │
       ╰─┴─┴─╯
      EtherChannel
       ╭─┬─┬─╮
       │ │ │ │
       SWITCH 2
```

Pot aportar:

- més capacitat agregada;
- redundància;
- un únic enllaç lògic per STP.

---

# 3.26. LACP

LACP és un protocol estàndard (IEEE 802.1AX, històricament associat a 802.3ad) per negociar agregacions d’enllaços.

Exemple Cisco:

```
interface range gi0/21 - 22

channel-group 1 mode active
```

Després:

```
interface port-channel 1

switchport mode trunk

switchport trunk allowed vlan 10,20,30
```

Comprovar:

```
show etherchannel summary
```

---

# 3.27. Guardar configuració

Igual que al router:

```
show running-config
```

Configuració guardada:

```
show startup-config
```

Guardar:

```
copy running-config startup-config
```

També, en plataformes que ho admetin:

```
write memory
```

---

# 3.28. Comandes essencials de switch

|Comanda|Funció|
|---|---|
|`show interfaces status`|Estat ràpid dels ports|
|`show interfaces`|Informació detallada|
|`show interfaces description`|Descripcions|
|`show vlan brief`|VLAN i ports|
|`show interfaces trunk`|Trunks|
|`show mac address-table`|Taula MAC|
|`show spanning-tree`|Estat STP|
|`show etherchannel summary`|EtherChannel|
|`show power inline`|PoE|
|`show port-security`|Port Security|
|`show running-config`|Configuració actual|
|`show startup-config`|Configuració guardada|
|`show ip interface brief`|Resum d’interfícies/SVI|
|`ping`|Provar connectivitat|
|`copy running-config startup-config`|Guardar|

Aquest bloc és dels que tindria **a mà durant les pràctiques**.

---

# 3.29. Diagnosticar un port que no funciona

Suposem:

```
PC
 │
Gi0/7
 │
SWITCH
 │
ROUTER
```

El PC no té xarxa.

Podem seguir:

```
1. Comprovar cable
        ↓
2. Comprovar LED del port
        ↓
3. show interfaces status
        ↓
4. Comprovar si està shutdown
        ↓
5. show vlan brief
        ↓
6. Comprovar VLAN del port
        ↓
7. show mac address-table
        ↓
8. Comprovar trunk/uplink
        ↓
9. Comprovar DHCP
        ↓
10. Comprovar gateway
```

# Exemple d’instal·lació empresarial

Podríem tenir:

```
                       INTERNET
                           │
                        ROUTER
                           │
                     TRUNK 10,20,30
                           │
                     ┌─────▼─────┐
                     │  SWITCH   │
                     └─────┬─────┘
          ┌────────────────┼────────────────┐
          │                │                │
       VLAN 10          VLAN 20          TRUNK
        ADMIN           EMPLEATS            │
          │                │                AP
       PC-01            PC-02        ┌──────┴──────┐
                                  SSID EMPRESA   SSID GUEST
                                    VLAN 20       VLAN 30
```

Aquí ja comencem a unir **tot el que estem estudiant**:

**Router:** routing, DHCP, NAT i Internet.

**Switch:** VLAN, access, trunk i PoE.

**AP:** SSID, Wi-Fi, VLAN i seguretat.

**PC:** IP, DHCP, DNS i gateway.

Això és important perquè a la vida real **no diagnosticaràs els dispositius de manera completament independent**. Si un port funciona però el PC no rep IP, per exemple, el problema pot estar al switch, al trunk, a la VLAN, al DHCP o al router.