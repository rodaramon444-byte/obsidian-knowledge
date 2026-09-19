![[Pasted image 20260919124713.png]]
## Què és un router?

Un **router** és un dispositiu de xarxa encarregat de comunicar **xarxes diferents**.

En una instal·lació típica:

```
                  INTERNET
                     │
                     │ WAN
                     ▼
               ┌──────────┐
               │  ROUTER  │
               └────┬─────┘
                    │ LAN
                    │
              ┌─────▼─────┐
              │  SWITCH   │
              └──┬──┬──┬──┘
                 │  │  │
                PC  PC  AP
```

El router acostuma a actuar com a **porta d’enllaç (default gateway)** dels dispositius de la xarxa.

Per exemple:

```
Router:       192.168.1.1
PC-01:        192.168.1.10
PC-02:        192.168.1.11
Impressora:   192.168.1.20
```

Els ordinadors utilitzarien:

```
Gateway: 192.168.1.1
```

---

# 2.2. WAN i LAN

És fonamental diferenciar-les.

### WAN

La interfície **WAN** connecta el router amb una altra xarxa, habitualment amb l'operador/Internet.

```
Internet
   │
   │ WAN
   ▼
 Router
```

### LAN

La **LAN** és la xarxa interna.

```
Router
   │
   │ LAN
   ▼
Switch
 ├── PC
 ├── PC
 ├── Impressora
 └── AP
```

Per tant:

```
WAN → exterior
LAN → xarxa interna
```

---

# 2.3. Instal·lació física

Abans de configurar el router, comprovar:

- Alimentació.
- Cable WAN.
- Cables LAN.
- LEDs d'estat.
- Velocitat dels ports.
- Estat físic dels connectors.
- Connexió amb switch/ONT/mòdem.

Una instal·lació típica amb fibra podria ser:

```
Fibra
  │
  ▼
 ONT
  │
Ethernet
  │
  ▼
Router
  │
Ethernet
  ▼
Switch
```

Depenent de l'operador i del router, l'ONT pot estar integrada en un altre dispositiu.

---

# 2.4. Accedir al router

Hi ha diverses formes.

### Interfície web

És habitual en routers de petites empreses.

Per exemple:

```
PC
 │
 └──── Ethernet ──── Router
```

Primer consultem la configuració:

```
ipconfig
```

Podríem obtenir:

```
IPv4:    192.168.1.25
Gateway: 192.168.1.1
```

Normalment, la IP del gateway és un bon primer lloc per comprovar si hi ha la interfície d'administració del router.

---

## SSH

En equips professionals és habitual administrar-los per SSH.

Exemple:

```
ssh admin@192.168.1.1
```

SSH permet administrar el router de manera xifrada.

---

## Consola

En dispositius professionals també podem connectar-nos directament:

```
PORTÀTIL
   │
USB / consola
   │
   ▼
 ROUTER
```

És especialment útil durant la configuració inicial, quan el router encara no té una IP de gestió accessible.

---

# 2.5. Configuració inicial d'un router Cisco IOS

Entrar al mode privilegiat:

```
Router> enable
Router#
```

Entrar a configuració:

```
Router# configure terminal
Router(config)#
```

---

## Canviar hostname

```
Router(config)# hostname R1
```

Ara:

```
R1(config)#
```

En una empresa podríem utilitzar noms més descriptius:

```
RTR-OFICINA-01
RTR-SEU-TGN-01
RTR-MAGATZEM-01
```

---

# 2.6. Configurar una interfície

Primer podem consultar les interfícies:

```
R1# show ip interface brief
```

Exemple:

```
Interface              IP-Address      Status      Protocol

GigabitEthernet0/0     unassigned      down        down
GigabitEthernet0/1     unassigned      down        down
```

Suposem:

```
G0/0 → WAN
G0/1 → LAN
```

Configurem LAN:

```
R1# configure terminal

R1(config)# interface gigabitEthernet 0/1

R1(config-if)# ip address 192.168.1.1 255.255.255.0

R1(config-if)# no shutdown
```

`no shutdown` és molt important perquè activa administrativament la interfície.

Comprovem:

```
R1# show ip interface brief
```

Voldríem veure:

```
GigabitEthernet0/1   192.168.1.1   YES manual   up   up
```

---

# 2.7. Configurar DHCP

El router pot actuar com a servidor DHCP.

Suposem aquesta xarxa:

```
Xarxa:       192.168.1.0/24
Router:      192.168.1.1
DHCP:        192.168.1.50 - 192.168.1.200
```

Primer excloem les IP que no volem entregar automàticament:

```
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.49
```

Creem el pool:

```
R1(config)# ip dhcp pool LAN
```

Definim la xarxa:

```
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
```

Gateway:

```
R1(dhcp-config)# default-router 192.168.1.1
```

DNS, per exemple:

```
R1(dhcp-config)# dns-server 1.1.1.1 8.8.8.8
```

Quedaria:

```
PC
 │
 │ DHCP
 ▼
Router
 │
 ├── IP:      192.168.1.50-200
 ├── Mask:    255.255.255.0
 ├── Gateway: 192.168.1.1
 └── DNS:     configurat al pool
```

---

# 2.8. Comprovar DHCP

Al router:

```
show ip dhcp binding
```

Ens permet veure concessions DHCP.

També:

```
show ip dhcp pool
```

Al PC Windows:

```
ipconfig /release
ipconfig /renew
ipconfig /all
```

Comprovem que hagi rebut:

```
IPv4:     192.168.1.X
Mask:     255.255.255.0
Gateway:  192.168.1.1
DNS:      ...
```

---

# 2.9. Configuració WAN

La configuració de la WAN dependrà de l'ISP i de la infraestructura.

Podem trobar:

```
DHCP
IP estàtica
PPPoE
VLAN de l'operador
```

Per exemple, si la WAN obté IP automàticament en un Cisco compatible amb aquesta configuració:

```
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address dhcp
R1(config-if)# no shutdown
```

Comprovem:

```
show ip interface brief
```

---

# 2.10. Ruta per defecte

El router necessita saber on enviar tràfic quan no té una ruta més específica.

Una ruta estàtica per defecte té la forma:

```
0.0.0.0/0
```

Per exemple, si el següent router és `10.0.0.1`:

```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

Conceptualment:

```
LAN
 │
 ▼
R1
 │
 ├── Conec aquesta xarxa? → envio segons taula
 │
 └── No la conec
         │
         ▼
     DEFAULT ROUTE
         │
         ▼
      Internet
```

Consultar rutes:

```
show ip route
```

---

# 2.11. NAT i PAT

Aquesta és una part molt important.

Els ordinadors de la LAN normalment utilitzen IP privades:

```
192.168.1.10
192.168.1.11
192.168.1.12
```

Aquestes adreces privades no es ruten directament per Internet.

El router pot fer **NAT/PAT** perquè múltiples dispositius interns comparteixin una IP pública.

```
PC1 192.168.1.10 ─┐
PC2 192.168.1.11 ─┼── ROUTER ── IP pública ── Internet
PC3 192.168.1.12 ─┘
```

Amb PAT, el router diferencia les connexions utilitzant també els ports.

---

# 2.12. NAT/PAT en Cisco IOS

Exemple simplificat.

Definim la interfície interna:

```
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip nat inside
```

Interfície exterior:

```
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip nat outside
```

Identifiquem la xarxa interna:

```
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
```

Activem PAT utilitzant la IP de la WAN:

```
R1(config)# ip nat inside source list 1 interface gigabitEthernet 0/0 overload
```

`overload` permet que múltiples dispositius comparteixin la mateixa IP exterior.

Comprovar traduccions:

```
show ip nat translations
```

Estadístiques:

```
show ip nat statistics
```

---

# 2.13. Port forwarding

Suposem que tenim un servei intern:

```
Servidor
192.168.1.100
```

i necessitem publicar un servei concret cap a l'exterior.

Conceptualment:

```
Internet
   │
IP pública:PORT
   │
   ▼
Router
   │
Port Forwarding
   │
   ▼
192.168.1.100:PORT
```

**Important:** no s'han d'obrir ports indiscriminadament. Només els necessaris, amb autorització i valorant abans alternatives com VPN, perquè estem exposant un servei a xarxes externes.

---

# 2.14. VLAN i router

Suposem una empresa amb:

```
VLAN 10 → Administració
VLAN 20 → Treballadors
VLAN 30 → Wi-Fi convidats
```

Podríem tenir:

```
VLAN 10 → 192.168.10.0/24
VLAN 20 → 192.168.20.0/24
VLAN 30 → 192.168.30.0/24
```

El router o un switch de capa 3 pot permetre comunicació entre aquestes xarxes, segons les regles establertes.

Això ens porta a una configuració molt habitual:

```
                   ROUTER
                      │
                    TRUNK
                      │
                      ▼
                   SWITCH
               ┌──────┼──────┐
               │      │      │
            VLAN10 VLAN20 VLAN30
               │      │      │
              PCs    PCs     AP
```

Aquesta part la desenvoluparem molt més quan fem el document de **switches**.

---

# 2.15. Router-on-a-stick

En Cisco podem crear subinterfícies.

Per exemple:

```
interface gigabitEthernet 0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
```

VLAN 20:

```
interface gigabitEthernet 0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

VLAN 30:

```
interface gigabitEthernet 0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

I la interfície física:

```
interface gigabitEthernet 0/1
 no shutdown
```

Ara:

```
192.168.10.1 → Gateway VLAN 10
192.168.20.1 → Gateway VLAN 20
192.168.30.1 → Gateway VLAN 30
```

---

# 2.16. Guardar la configuració

Molt important en Cisco.

La configuració actual està a:

```
running-config
```

Consultar:

```
show running-config
```

La configuració guardada:

```
show startup-config
```

Guardar els canvis:

```
copy running-config startup-config
```

També és habitual:

```
write memory
```

o:

```
wr
```

Si configures tot el router però **no guardes la configuració**, pots perdre els canvis quan es reiniciï.

---

# 2.17. Comandes Cisco essencials

Aquest seria un dels apartats que jo tindria més a mà durant les pràctiques.

|Comanda|Funció|
|---|---|
|`enable`|Mode privilegiat|
|`configure terminal`|Configuració global|
|`show running-config`|Configuració actual|
|`show startup-config`|Configuració guardada|
|`show ip interface brief`|Resum d'interfícies|
|`show interfaces`|Informació detallada|
|`show ip route`|Taula d'encaminament|
|`show arp`|Taula ARP|
|`show ip dhcp binding`|Clients DHCP|
|`show ip dhcp pool`|Pools DHCP|
|`show ip nat translations`|Traduccions NAT|
|`show ip nat statistics`|Estadístiques NAT|
|`ping`|Comprovar connectivitat|
|`traceroute`|Comprovar recorregut|
|`no shutdown`|Activar interfície|
|`shutdown`|Desactivar interfície|
|`copy running-config startup-config`|Guardar configuració|

---

# 2.18. Diagnòstic: no hi ha Internet

Igual que amb els ordinadors, és millor seguir un ordre.

```
1. Comprovar alimentació
        ↓
2. Comprovar cables i LEDs
        ↓
3. show ip interface brief
        ↓
4. Comprovar LAN
        ↓
5. Comprovar WAN
        ↓
6. show ip route
        ↓
7. Comprovar ruta per defecte
        ↓
8. ping al següent salt/gateway WAN
        ↓
9. ping a una IP d'Internet
        ↓
10. Comprovar DNS
        ↓
11. Comprovar NAT
        ↓
12. Comprovar ACL/firewall
```

Per exemple:

```
PC → Router = FUNCIONA
Router → Internet = NO FUNCIONA
```

Això ens indica que probablement **el problema no està entre el PC i la LAN**, sinó que hem de continuar investigant la WAN, routing, NAT, ISP, etc.

En canvi:

```
PC → Router = NO FUNCIONA
```

Primer investigarem:

```
IP del PC
Màscara
Gateway
Cable
Port del switch
VLAN
Interfície LAN del router
```

### Exemple amb ACL en un router Cisco

En Cisco IOS, una manera de filtrar trànsit és mitjançant **ACL (Access Control Lists)**. Per exemple, imagina que la VLAN de convidats és:

```
VLAN 30
192.168.30.0/24
```

i la xarxa corporativa és:

```
192.168.10.0/24
```

Volem que els convidats **no puguin entrar a la xarxa corporativa**, però sí que puguin continuar enviant altre trànsit:

```
configure terminal

ip access-list extended GUEST-FILTER

deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255

permit ip 192.168.30.0 0.0.0.255 any
```

Després s’aplica a la interfície/subinterfície adequada, per exemple:

```
interface GigabitEthernet0/1.30

ip access-group GUEST-FILTER in
```

Conceptualment:

```
Mòbil convidat
192.168.30.50
      │
      ▼
   VLAN 30
      │
      ▼
  FIREWALL/ACL
      │
      ├──► 192.168.10.0/24  ❌ DENY
      │
      └──► altres destins   ✓ PERMIT
```

Aquí has d’anar amb compte: una ACL mal aplicada pot deixar una part de l’empresa sense comunicació. A més, una ACL de router **no equival necessàriament a totes les funcions d’un firewall modern**.

### Firewalls empresarials reals

En empreses és habitual trobar dispositius/plataformes dedicades de fabricants com [Fortinet](https://www.fortinet.com/?utm_source=chatgpt.com), [Sophos](https://www.sophos.com/?utm_source=chatgpt.com), [Cisco](https://www.cisco.com/?utm_source=chatgpt.com), [Palo Alto Networks](https://www.paloaltonetworks.com/?utm_source=chatgpt.com) o solucions com [pfSense](https://www.pfsense.org/?utm_source=chatgpt.com).

En aquests casos normalment configures polítiques visualment:

```
POLÍTICA 1

ORIGEN:      LAN
DESTINACIÓ:  Internet
SERVEI:      HTTP/HTTPS/DNS
ACCIÓ:       ALLOW
```

Una altra:

```
POLÍTICA 2

ORIGEN:      GUEST
DESTINACIÓ:  LAN
SERVEI:      ANY
ACCIÓ:       DENY
```

I una altra:

```
POLÍTICA 3

ORIGEN:      Internet
DESTINACIÓ:  LAN
SERVEI:      ANY
ACCIÓ:       DENY
```

L’ordre de les regles és important en molts firewalls, perquè normalment es processen de dalt a baix fins que alguna coincideix.

### Exemple més realista

Suposem:

```
VLAN 10 ADMIN       → 192.168.10.0/24
VLAN 20 EMPLEATS    → 192.168.20.0/24
VLAN 30 GUEST       → 192.168.30.0/24
VLAN 40 SERVIDORS   → 192.168.40.0/24
```

Podríem dissenyar les polítiques aproximadament així:

|Origen|Destí|Servei|Acció|
|---|---|---|---|
|ADMIN|SERVIDORS|Necessaris|✅ Allow|
|EMPLEATS|SERVIDORS|Necessaris|✅ Allow|
|GUEST|SERVIDORS|ANY|❌ Deny|
|GUEST|ADMIN|ANY|❌ Deny|
|GUEST|EMPLEATS|ANY|❌ Deny|
|GUEST|Internet|HTTP/HTTPS/DNS|✅ Allow|
|Internet|LAN|No autoritzat|❌ Deny|

A partir d’aquí pots afegir controls molt més avançats: VPN, IDS/IPS, filtratge web, control d’aplicacions, logs, NAT, DMZ, etc.

Per a les teves pràctiques, jo aprendria especialment aquesta seqüència mental:

```
Qui inicia la connexió?
        ↓
Des de quina xarxa/VLAN?
        ↓
Cap a quina xarxa?
        ↓
Quin servei/port necessita?
        ↓
S'ha de permetre?
        ↓
ALLOW / DENY
        ↓
Registrar-ho als logs?
```

Per exemple: _«Els ordinadors de treballadors necessiten accedir al servidor web intern per HTTPS»_:

```
ORIGEN:       VLAN 20 EMPLEATS
DESTINACIÓ:   Servidor 192.168.40.10
PROTOCOL:     TCP
PORT:         443
ACCIÓ:        ALLOW
```

I **no** faries simplement:

```
EMPLEATS → SERVIDORS → ANY → ALLOW
```