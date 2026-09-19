![[Pasted image 20260919125123.png]]
## Què és un punt d’accés?

Un **Access Point (AP)** permet connectar dispositius sense fils a una xarxa Ethernet.

```
                 INTERNET
                     │
                  ROUTER
                     │
                  SWITCH
                     │
                 Ethernet
                     │
                     AP
                  )))  )))
                Wi-Fi
              /    |     \
         Portàtil Mòbil  Tablet
```

L’AP normalment **no substitueix el router**.

El router pot encarregar-se de routing, DHCP, NAT, firewall, etc., mentre que l’AP proporciona accés sense fils a la LAN.

---

# 4.2. Router Wi-Fi vs punt d’accés

És important diferenciar-los.

Un **router Wi-Fi domèstic** pot integrar en un mateix dispositiu:

```
Router
+
Switch
+
Access Point
+
DHCP
+
NAT
+
Firewall
```

Un **AP empresarial** acostuma a centrar-se en la connectivitat Wi-Fi:

```
                 ROUTER/FIREWALL
                       │
                     SWITCH
                       │
              ┌────────┼────────┐
              │        │        │
             AP1      AP2      AP3
```

Aquesta arquitectura permet donar cobertura Wi-Fi a oficines, hotels, centres educatius, empreses, etc.

---

# 4.3. Instal·lació física

La ubicació de l’AP és molt important.

Generalment interessa:

- Posició elevada.
- Zona relativament central.
- Evitar obstacles innecessaris.
- Evitar armaris metàl·lics.
- Evitar col·locar-lo darrere de grans obstacles.
- Tenir en compte parets i forjats.
- Evitar fonts importants d’interferències.
- Distribuir diversos AP adequadament.

Per exemple, és millor:

```
      ┌──────────────────────┐
      │                      │
      │          AP          │
      │        )))(((        │
      │                      │
      │                      │
      └──────────────────────┘
```

que tenir l’AP en una cantonada si volem cobrir tota l’estança:

```
      ┌──────────────────────┐
      │ AP                   │
      │ )))                  │
      │                      │
      │                      │
      └──────────────────────┘
```

La ubicació òptima real depèn de l’edifici, materials, clients, interferències i disseny de cobertura.

---

# 4.4. Connexió de l’AP

Normalment:

```
AP
 │
 │ Ethernet
 ▼
SWITCH
```

Però molts AP empresarials utilitzen **PoE**.

```
             SWITCH PoE
                 │
                 │ dades + alimentació
                 │
                 ▼
                 AP
```

Això permet utilitzar un únic cable Ethernet per transportar:

```
DADES + ALIMENTACIÓ
```

---

# 4.5. PoE

Ens podem trobar diferents estàndards, com ara:

```
IEEE 802.3af  → PoE
IEEE 802.3at  → PoE+
IEEE 802.3bt  → PoE de major potència
```

No tots els AP tenen els mateixos requisits.

Per tant, abans de connectar un AP cal comprovar:

```
Quin estàndard PoE necessita l'AP?
            ↓
El switch el suporta?
            ↓
Hi ha pressupost PoE suficient?
```

En Cisco, segons el model:

```
show power inline
```

Ens pot ajudar a comprovar l’estat PoE.

---

# 4.6. Injector PoE

Si el switch no proporciona PoE, en alguns casos podem utilitzar un **injector PoE compatible**.

```
SWITCH
   │
   │ Ethernet
   ▼
┌──────────────┐
│ Injector PoE │──── alimentació
└──────┬───────┘
       │
       │ dades + alimentació
       ▼
       AP
```

Cal comprovar sempre la compatibilitat entre injector i dispositiu.

---

# 4.7. Accedir a l’AP

Depenent del fabricant, podem configurar-lo mitjançant:

```
Interfície web
Aplicació
Controlador
Cloud
CLI/SSH
```

Per exemple:

```
PC
 │
 ▼
Controlador
 │
 ├── AP1
 ├── AP2
 ├── AP3
 └── AP4
```

Això és molt habitual en instal·lacions empresarials.

---

# 4.8. AP autònom vs controlador

### AP autònom

Configurem cada AP individualment.

```
AP1 → configuració
AP2 → configuració
AP3 → configuració
```

Pot ser suficient per instal·lacions petites.

### AP gestionats centralment

Tenim una plataforma/controlador:

```
             CONTROLADOR
                 │
       ┌─────────┼─────────┐
       │         │         │
      AP1       AP2       AP3
```

Des d’un únic lloc podem gestionar:

```
SSID
VLAN
Canals
Potència
Clients
Firmware
Estadístiques
Roaming
```

És molt més pràctic quan hi ha molts AP.

---

# 4.9. SSID

L’**SSID** és el nom visible de la xarxa Wi-Fi.

Per exemple:

```
EMPRESA
EMPRESA-GUEST
MAGATZEM
```

Podem tenir diferents SSID sobre els mateixos AP:

```
                 AP
            )))      )))
             │        │
             │        │
        EMPRESA    CONVIDATS
```

---

# 4.10. Seguretat Wi-Fi

En entorns moderns ens podem trobar principalment amb:

```
WPA2
WPA3
```

Cal evitar tecnologies antigues i insegures com WEP.

Una configuració senzilla podria ser:

```
SSID: EMPRESA
Seguretat: WPA2/WPA3
Contrasenya: ********
```

Però en una empresa també podem trobar autenticació empresarial amb **802.1X/RADIUS**, on cada usuari/dispositiu pot autenticar-se amb credencials o certificats en lloc de compartir una única contrasenya.

---

# 4.11. Contrasenya Wi-Fi

Una xarxa amb PSK hauria d’utilitzar una contrasenya robusta.

Evitar:

```
empresa123
12345678
password
wifi2026
```

És preferible una contrasenya llarga, aleatòria o una passphrase robusta segons la política de l’organització.

A més, la xarxa de convidats no hauria de proporcionar accés innecessari a la xarxa corporativa.

---

# 4.12. VLAN per SSID

Aquesta és probablement **la part més important del document** juntament amb canals i cobertura.

Imaginem:

```
SSID EMPRESA
      ↓
VLAN 20
      ↓
192.168.20.0/24
```

i:

```
SSID CONVIDATS
      ↓
VLAN 30
      ↓
192.168.30.0/24
```

L’AP anuncia les dues xarxes:

```
                    AP
               )))      )))
                │        │
             EMPRESA   GUEST
                │        │
             VLAN20    VLAN30
```

---

# 4.13. Connexió AP → switch

Si l’AP transporta múltiples VLAN, el port del switch sovint haurà de transportar-les mitjançant un **trunk**.

```
SSID EMPRESA ── VLAN 20 ─┐
                         │
SSID GUEST ──── VLAN 30 ─┤
                         │
                         AP
                         │
                       TRUNK
                         │
                       SWITCH
```

En Cisco, segons el disseny:

```
interface gi0/10
 description AP-PLANTA-1
 switchport mode trunk
 switchport trunk allowed vlan 20,30
```

Ara ja veus per què era tan important entendre els trunks al punt anterior.

---

# 4.14. Xarxa de convidats

Una xarxa Guest podria tenir:

```
SSID: EMPRESA-GUEST
VLAN: 30
Xarxa: 192.168.30.0/24
```

L’objectiu habitual és:

```
CONVIDAT
   │
 Wi-Fi
   │
VLAN 30
   │
   ├──► Internet       ✓
   │
   └──► Xarxa interna  ✕
```

L’aïllament real s’ha d’implementar mitjançant les polítiques adequades al router/firewall.

No n’hi ha prou simplement amb posar-li un SSID diferent.

---

# 4.15. Bandes Wi-Fi

Actualment és habitual treballar amb:

```
2,4 GHz
5 GHz
6 GHz
```

La disponibilitat de 6 GHz dependrà dels AP, clients, estàndard Wi-Fi i regulació aplicable.

### 2,4 GHz

Avantatges:

```
Més abast
Millor penetració d'obstacles
```

Inconvenients:

```
Menys espectre disponible
Més interferències habituals
Menys capacitat que bandes superiors
```

### 5 GHz

Normalment proporciona:

```
Més canals
Més capacitat
Menys congestió que 2,4 GHz en molts entorns
```

però acostuma a tenir menys abast/penetració que 2,4 GHz.

### 6 GHz

Pot proporcionar molt més espectre per a tecnologies compatibles, però:

```
Requereix equips compatibles
Té característiques de propagació diferents
```

---

# 4.16. Canals Wi-Fi

Els AP transmeten sobre **canals**.

En 2,4 GHz, en moltes configuracions tradicionals s’utilitzen els canals:

```
1
6
11
```

per evitar solapaments en desplegaments amb amplada de 20 MHz en regions on aquests canals són disponibles.

Exemple:

```
AP1          AP2          AP3
 │            │            │
CH1          CH6          CH11
```

No convé configurar tots els AP pròxims exactament igual sense estudiar l’entorn.

---

# 4.17. Interferències

Imaginem:

```
AP1 → Canal 1
AP2 → Canal 1
AP3 → Canal 1
```

Si estan molt pròxims, poden competir pel mateix temps d’aire.

Un disseny millor podria distribuir els canals:

```
AP1 → CH1
AP2 → CH6
AP3 → CH11
```

però la configuració òptima depèn de:

- xarxes veïnes;
- potència;
- nombre d’AP;
- clients;
- amplada de canal;
- obstacles;
- bandes disponibles.

---

# 4.18. Amplada de canal

Ens podem trobar:

```
20 MHz
40 MHz
80 MHz
160 MHz
```

Una amplada superior pot permetre més velocitat, però consumeix més espectre.

Per tant:

```
Canal més ample
      ↓
Potencialment més velocitat
      ↓
Però menys canals independents disponibles
```

En entorns amb molts AP, utilitzar sempre el canal més ample **no necessàriament és la millor configuració**.

---

# 4.19. Potència de transmissió

Un error habitual és pensar:

> «Com més potència, millor Wi-Fi.»

No necessàriament.

Si tenim:

```
AP1 ))))))))))))))))

AP2 ))))))))))))))))
```

amb cobertures excessives, podem augmentar interferències i dificultar que alguns clients canviïn d’AP.

Pot ser preferible:

```
AP1 )))))))

       AP2 )))))))
```

amb un disseny de cel·les adequat.

---

# 4.20. RSSI

El **RSSI** ens dona una indicació de la potència del senyal rebut.

Normalment es representa en **dBm**.

Exemple orientatiu:

```
-30 dBm → senyal molt fort
-50 dBm → fort
-60 dBm → generalment bo
-70 dBm → més feble
-80 dBm → molt feble
```

Com més ens apropem a `0`, més fort és el senyal.

Per exemple:

```
-45 dBm
```

és més fort que:

```
-75 dBm
```

Els llindars adequats depenen de l’aplicació, client i disseny.

---

# 4.21. Roaming

Imaginem una oficina:

```
┌─────────────────────────────────┐
│                                 │
│ AP1          AP2          AP3   │
│ )))          )))          )))   │
│                                 │
└─────────────────────────────────┘
```

Una persona camina amb un portàtil:

```
AP1 → AP2 → AP3
```

El **roaming** permet que el client canviï entre punts d’accés mantenint la connectivitat amb la mateixa xarxa de la manera més fluida possible.

Normalment els AP comparteixen:

```
Mateix SSID
Mateixa política de seguretat
Configuració coherent
```

i poden utilitzar mecanismes addicionals per facilitar el roaming.

---

# 4.22. 802.11k, 802.11v i 802.11r

En xarxes empresarials podem trobar:

**802.11k:** ajuda el client a conèixer AP/canals veïns.

**802.11v:** permet mecanismes d’assistència/gestió perquè el client prengui millors decisions de transició.

**802.11r:** permet **Fast BSS Transition**, reduint part del temps d’autenticació durant determinats roamings.

La compatibilitat depèn tant dels AP com dels clients.

---

# 4.23. Ubiquiti UniFi

És possible que durant les pràctiques et trobis ecosistemes gestionats com [Ubiquiti UniFi](https://ui.com/?utm_source=chatgpt.com).

La idea general és:

```
UniFi Controller
      │
      ├── Gateway
      ├── Switch
      ├── AP1
      ├── AP2
      └── AP3
```

Des del sistema de gestió podem centralitzar bona part de la configuració i monitorització.

---

# 4.24. TP-Link Omada

Un altre ecosistema empresarial habitual és [TP-Link Omada](https://www.omadanetworks.com/?utm_source=chatgpt.com).

Conceptualment funciona de manera similar:

```
Omada Controller
      │
      ├── Gateway
      ├── Switch
      ├── AP
      └── AP
```

La configuració exacta dependrà del model i de la versió del controlador.

---

# 4.25. Comprovar el Wi-Fi des de Windows

Podem consultar informació amb:

```
netsh wlan show interfaces
```

Ens permet veure dades com:

```
SSID
BSSID
Canal
Tipus de ràdio
Velocitats
Senyal
```

Veure xarxes:

```
netsh wlan show networks mode=bssid
```

Això pot ser molt útil per diagnosticar:

```
Quines xarxes detecto?
Quin canal utilitzen?
Quins BSSID hi ha?
Quin senyal tinc?
```

---

# 4.26. Comprovar el Wi-Fi des de Linux

Una comanda molt útil és:

```
nmcli device wifi list
```

També:

```
nmcli connection show
```

Informació de les interfícies:

```
ip addr
```

Rutes:

```
ip route
```

Per exemple:

```
nmcli device wifi list
```

pot ajudar-nos a veure SSID, canal, senyal i seguretat de les xarxes detectades.

---

# 4.27. Problema: «Em connecto al Wi-Fi però no tinc Internet»

Aquesta incidència és molt habitual.

Cal separar:

```
CONNECTAR-SE AL WI-FI
        ≠
TENIR INTERNET
```

Podem estar correctament associats a l’AP però tenir un problema posterior.

Procediment:

```
1. Estic connectat al SSID correcte?
            ↓
2. Tinc una IP?
            ↓
3. La IP correspon a la VLAN correcta?
            ↓
4. Tinc gateway?
            ↓
5. Tinc DNS?
            ↓
6. Puc fer ping al gateway?
            ↓
7. Puc arribar a una IP externa?
            ↓
8. Funciona DNS?
            ↓
9. VLAN/trunk correctes?
            ↓
10. DHCP/router/firewall correctes?
```

---

# 4.28. Exemple real

Un usuari es connecta a:

```
EMPRESA
```

però rep:

```
169.254.52.34
```

Això ens fa sospitar que **no ha obtingut correctament una concessió DHCP**.

Investiguem:

```
CLIENT
  │
 Wi-Fi
  ▼
 AP
  │
 VLAN 20
  ▼
 SWITCH
  │
 TRUNK
  ▼
 ROUTER / SERVIDOR DHCP
```

Podria haver-hi, per exemple:

```
SSID associat a VLAN incorrecta
VLAN no permesa al trunk
Port del switch mal configurat
Problema amb DHCP
Problema de relay DHCP
Problema de routing
```

No necessàriament és un problema de cobertura Wi-Fi.

---

# 4.29. Problema: «El Wi-Fi va lent»

No comencem reiniciant coses aleatòriament.

Comprovem:

```
1. Senyal / RSSI
        ↓
2. Banda utilitzada
        ↓
3. Canal
        ↓
4. Interferències
        ↓
5. Amplada del canal
        ↓
6. Nombre de clients
        ↓
7. Utilització del canal
        ↓
8. Enllaç Ethernet de l'AP
        ↓
9. PoE
        ↓
10. Switch
        ↓
11. Router / WAN
        ↓
12. Internet
```

Així podem determinar si el problema és realment:

```
Wi-Fi
LAN
WAN
Internet
DNS
Servidor/aplicació
```

---

# 4.30. Problema: poca cobertura

Comprovar:

```
Ubicació de l'AP
        ↓
Obstacles
        ↓
Banda
        ↓
Potència
        ↓
Interferències
        ↓
Canals
        ↓
Densitat d'AP
```

La solució no és necessàriament augmentar la potència.

En alguns casos la solució correcta pot ser **instal·lar un altre AP i redissenyar la cobertura**.

---

# 4.31. Problema: un AP no s’encén

Si utilitza PoE:

```
AP no s'encén
     ↓
Comprovar cable
     ↓
Comprovar port
     ↓
Comprovar PoE
     ↓
Comprovar estàndard compatible
     ↓
Comprovar pressupost PoE del switch
     ↓
Provar altre cable/port
     ↓
Comprovar AP
```

En Cisco:

```
show power inline
```

pot ser una de les primeres comprovacions.

---

# 4.32. Exemple complet d’una xarxa empresarial

Ara ja podem unir els **quatre documents**:

```
                         INTERNET
                             │
                             │
                       ┌─────▼─────┐
                       │  ROUTER   │
                       │           │
                       │ NAT       │
                       │ DHCP      │
                       │ ROUTING   │
                       └─────┬─────┘
                             │
                    TRUNK 10,20,30
                             │
                       ┌─────▼─────┐
                       │  SWITCH   │
                       └─────┬─────┘
              ┌──────────────┼──────────────┐
              │              │              │
           ACCESS         ACCESS          TRUNK
           VLAN10         VLAN20          20,30
              │              │              │
          PC ADMIN      PC EMPLEAT          AP
                                         )))  )))
                                          │    │
                                       EMPRESA GUEST
                                       VLAN20 VLAN30
```

Les xarxes podrien ser:

```
VLAN 10 → ADMIN
192.168.10.0/24

VLAN 20 → EMPLEATS
192.168.20.0/24

VLAN 30 → CONVIDATS
192.168.30.0/24
```

Aquesta topologia és molt interessant perquè quan apareix una incidència pots anar **seguint físicament i lògicament el recorregut de la connexió**.