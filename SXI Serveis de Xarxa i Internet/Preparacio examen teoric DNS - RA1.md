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

UF1NF1.Instal·lació i Configura…

**Això té molta pinta de pregunta d'examen.**