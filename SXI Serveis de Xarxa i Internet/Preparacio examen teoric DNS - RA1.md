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