![[Pasted image 20261001205957.png]]
## BLOC 1 — Què és DNS?

**DNS = Domain Name System.**

La seva funció principal és relacionar **noms de domini amb informació de xarxa**, especialment adreces IP.
Per exemple:

```
www.informatica.com
        ↓ DNS
192.168.10.10
```

# BLOC 2 — Espai de noms DNS

Això és **molt important**.

DNS té una estructura **jeràrquica en forma d'arbre invertit**. dns

Imagina:

```
                 .
              ARREL
           /     |     \
         com    org    net
          |
     informatica
          |
         asix
          |
         www
```

Un nom complet podria ser:

```
www.asix.informatica.com.
```

I es llegeix conceptualment **de dreta a esquerra**:

```
.             → arrel
com           → TLD
informatica   → domini
asix          → subdomini
www           → host
```

### FQDN

**FQDN = Fully Qualified Domain Name**

És el **nom complet d'un host** dins de l'arbre DNS.

```
www.informatica.com.
```

⚠️ El **punt final** és important: representa l'arrel DNS.

```
www.informatica.com.   ← nom absolut / FQDN
www                    ← nom relatiu
```

En BIND, si oblides el punt final en determinats noms, BIND pot considerar que és un nom relatiu i afegir-hi el domini de la zona.

De fet, els apunts del professor remarquen aquest error:

```
www.iesebre.com
```

sense punt final podria interpretar-se com:

```
www.iesebre.com.iesebre.com
```


# BLOC 3 — Resolució directa i inversa

Has de diferenciar-les perfectament.

### Directa

```
NOM → IP
```

Exemple:

```
www.informatica.com
        ↓
192.168.10.10
```

Principalment utilitza:

```
A     → IPv4
AAAA  → IPv6
```

### Inversa

```
IP → NOM
```

Exemple:

```
192.168.10.10
       ↓
www.informatica.com
```

Utilitza:

```
PTR
```

Per IPv4 s'utilitza:

```
in-addr.arpa
```

Per exemple:

```
192.168.10.0/24
```

es transforma en:

```
10.168.192.in-addr.arpa
```

dns

**Truc per memoritzar-ho:**

```
A    = nom → IP
PTR  = IP → nom
```

# BLOC 4 — Registres DNS

Això és probablement **una de les parts més importants de l'examen**.

|Registre|Què fa|
|---|---|
|**A**|Nom → IPv4|
|**AAAA**|Nom → IPv6|
|**CNAME**|Crea un àlies|
|**NS**|Indica el servidor DNS autoritatiu|
|**SOA**|Informació principal de la zona|
|**PTR**|IP → nom|
|**MX**|Servidor de correu|
|**TXT**|Informació textual|
|**SRV**|Localització d'un servei|
Exemples:

```
www    IN A       192.168.10.10
www    IN AAAA    2001:db8::10
ftp    IN CNAME   www
@      IN NS      ns1.informatica.com.
10     IN PTR     www.informatica.com.
@      IN MX 10   mail.informatica.com.
```

### CNAME

És un **àlies**.

```
www IN A 192.168.10.10
ftp IN CNAME www
```

Per tant:

```
ftp.informatica.com
        ↓
www.informatica.com
        ↓
192.168.10.10
```

Important:

> **CNAME apunta a un nom, no directament a una IP.**

# BLOC 5 — SOA

**SOA = Start of Authority**

És el registre que conté la informació principal de la zona.

Exemple:

```
@ IN SOA ns1.informatica.com. admin.informatica.com. (
    2024092501
    3600
    900
    604800
    86400
)
```

Has de saber què significa:

```
Servidor principal → servidor principal de la zona
Contacte           → administrador
Serial             → versió de la zona
Refresh            → quan el secundari comprova canvis
Retry              → quan torna a intentar-ho si falla
Expire             → quan la informació deixa de ser vàlida
Negative TTL       → caché de respostes negatives
```

### SERIAL

Aquesta és **importantíssima**.

Cada vegada que modifiques una zona has d'**incrementar el serial**.

Per exemple:

```
2026100101
       ↓
2026100102
       ↓
2026100103
```

El servidor secundari utilitza el serial per saber si la informació del primari ha canviat. Els apunts també indiquen que és habitual utilitzar el format:

```
AAAAMMDDNN
```

# BLOC 6 — Zones DNS

Una **zona** és una part de l'espai DNS administrada com una unitat.

Per exemple:

```
informatica.com
```

pot ser una zona.

Tenim dues especialment importants:

```
ZONA DIRECTA
nom → IP
A / AAAA

ZONA INVERSA
IP → nom
PTR
```

# LOC 7 — BIND9

**BIND9 (Berkeley Internet Name Domain)** és una implementació de servidor DNS utilitzada en Linux/Unix. 

El procés del servidor s'anomena:

```
named
```

I els fitxers principals que heu treballat són:

```
/etc/bind/
│
├── named.conf
├── named.conf.options
├── named.conf.local
├── named.conf.default-zones
│
└── db.*
```

Memoritza especialment:

```
named.conf
→ configuració principal

named.conf.options
→ opcions generals

named.conf.local
→ definició de les nostres zones

db.*
→ registres/dades de les zones
```

# BLOC 8 — Forwarders

Un **forwarder** és un servidor DNS al qual el nostre DNS envia les consultes que no pot resoldre.

```
CLIENT
   ↓
BIND9
   ↓
FORWARDER
   ↓
INTERNET
```

Exemple:

```
forwarders {
    1.1.1.1;
    8.8.8.8;
};
```

En els apunts antics també apareix:

```
forward only;
```

Això significa que les consultes externes **només s'envien als forwarders**.

# BLOC 9 — DNS primari i secundari

### Primari / mestre

Té la **còpia principal i editable de la zona**.

### Secundari / esclau

Rep una còpia de la zona del primari.

Això proporciona redundància i pot ajudar a distribuir la càrrega: si el primari deixa de funcionar, el secundari pot continuar oferint el servei.

Les transferències poden ser:

```
AXFR → transferència completa
IXFR → transferència incremental
```

I aquí torna a ser important el:

```
SERIAL
```

Si el secundari detecta un serial superior al primari, sap que ha d'actualitzar la seva informació.

# BLOC 10 — Comandes que has de reconèixer

Jo aquestes me les aprendria sí o sí:

```
named-checkconf
```

Comprova la configuració de BIND.

```
named-checkzone
```

Comprova una zona.

```
dig
```

Fa consultes DNS i serveix per diagnosticar.

```
host
```

Resol noms/IP de manera senzilla.

```
nslookup
```

Una altra eina per consultar DNS.

```
rndc status
```

Consulta l'estat de BIND.

I a la presentació nova apareix el flux que hauríeu de seguir després de modificar una zona:

```
EDITAR ZONA
     ↓
INCREMENTAR SERIAL
     ↓
named-checkzone
     ↓
named-checkconf
     ↓
rndc reload
     ↓
dig
```

