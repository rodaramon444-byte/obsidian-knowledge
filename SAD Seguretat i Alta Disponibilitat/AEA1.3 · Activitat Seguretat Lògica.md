## 1. Confidencialitat – EFS

**Creació de l'usuari de proves**  
S'ha creat l'usuari local `usuari_efs` mitjançant la comanda `net user`. Posteriorment s'ha comprovat la seva configuració i s'ha verificat que pertany únicament al grup local **Usuarios**, per tant, no disposa de privilegis d'administrador. Aquest compte serà l'utilitzat per realitzar el xifratge EFS.
![[Pasted image 20261016154223.png]]

**Preparació de les dades de prova**  
Des del compte `usuari_efs` s'ha creat la carpeta `Documents_EFS` a l'Escriptori. Dins d'aquesta carpeta s'han creat dos documents de text que serviran per comprovar posteriorment el funcionament del xifratge EFS.
![[Pasted image 20261016154623.png]]

**Inici de sessió amb l'usuari no administrador**  
S'ha iniciat sessió amb el compte local `usuari_efs`. Mitjançant la comanda `whoami` s'ha verificat que la sessió actual correspon efectivament a aquest usuari. A partir d'aquest compte es realitzaran les proves de xifratge mitjançant EFS.
![[Pasted image 20261016155335.png]]

> **Xifratge de la carpeta mitjançant EFS**  
> S'ha activat el xifratge EFS sobre la carpeta `Documents_EFS` i el seu contingut des del compte no administrador `usuari_efs`. Posteriorment, s'ha utilitzat la comanda `cipher` per comprovar l'estat del xifratge. El resultat mostra la lletra `E` davant de `Documents_EFS`, indicant que la carpeta està xifrada mitjançant EFS.
![[Pasted image 20261016155500.png]]

![[Pasted image 20261016155651.png]]

**Accés des d'un compte administrador**  
S'ha tancat la sessió de `usuari_efs` i s'ha iniciat sessió amb l'usuari `rodar`. Mitjançant les comandes `whoami` i `net localgroup Administradores` s'ha comprovat que aquest compte forma part del grup local d'administradors. A continuació es comprovarà si el fet de disposar de privilegis administratius permet accedir a les dades protegides amb EFS.
![[Pasted image 20261016160005.png]]

**Prova de lectura des de l'usuari administrador**  
Des del compte administrador `rodar` s'ha accedit a la ubicació de la carpeta `Documents_EFS` i s'ha intentat obrir el fitxer `Document1.txt`. El sistema ha denegat l'accés i ha mostrat el missatge _“No tiene permiso para abrir este archivo”_.

Per tant, disposar de privilegis d'administrador no és suficient per llegir directament el contingut xifrat mitjançant EFS, ja que l'accés a les dades xifrades requereix la credencial EFS corresponent
![[Pasted image 20261016160218.png]]

**Prova de modificació dels atributs**  
Des del compte administrador `rodar` s'ha accedit a les propietats de `Document1.txt` i s'ha modificat l'atribut del fitxer, activant l'opció de només lectura. A diferència de la lectura del contingut, aquesta operació s'ha pogut realitzar sense errors. Per tant, EFS impedeix l'accés al contingut xifrat, però això no implica necessàriament que l'administrador no pugui efectuar determinades operacions sobre les metadades o atributs del fitxer.
![[Pasted image 20261016160508.png]]

**Prova de còpia del fitxer xifrat**  
En intentar copiar `Document1.txt` des de la carpeta de `usuari_efs` cap a l'Escriptori de l'administrador `rodar`, Windows informa que són necessaris permisos d'administrador per continuar amb l'operació. Això correspon als permisos d'accés a la ubicació del fitxer, per la qual cosa es continuarà l'operació per comprovar posteriorment si EFS permet copiar i accedir al contingut xifrat.
![[Pasted image 20261016160654.png]]

En intentar copiar `Document1.txt` a l'Escriptori de l'usuari administrador `rodar`, Windows ha sol·licitat inicialment privilegis d'administrador. Després de seleccionar l'opció **Continuar**, l'operació ha estat novament denegada i el sistema ha indicat que es requereixen permisos de l'usuari `usuari_efs`.

Per tant, en la configuració provada, l'administrador no ha pogut copiar el fitxer protegit. Aquesta prova, juntament amb l'intent de lectura anterior, mostra que disposar de privilegis administratius no implica automàticament poder accedir al contingut protegit per EFS.
![[Pasted image 20261016160730.png]]

**Comprovació del fitxer ZIP**  
Després de xifrar la carpeta `Documents_EFS` mitjançant EFS, s'ha creat un fitxer ZIP des del compte `usuari_efs`. Posteriorment s'ha iniciat sessió amb l'usuari administrador `rodar` i s'ha accedit al contingut del ZIP.

A diferència dels fitxers originals protegits amb EFS, l'administrador ha pogut obrir `Document1.txt` des del ZIP i llegir-ne el contingut sense cap error. A més, l'Explorador de Windows mostra que els fitxers continguts al ZIP no estan protegits.

**Conclusió:** en aquesta prova, la compressió a ZIP ha provocat que la còpia dels documents emmagatzemada dins del fitxer comprimit no mantingui el xifratge EFS. Per tant, encara que els fitxers originals continuïn protegits, el ZIP creat des del compte autoritzat conté còpies accessibles sense la protecció EFS. Això demostra que comprimir dades xifrades amb EFS d'aquesta manera pot generar una còpia que perdi aquesta protecció i suposar un risc per a la confidencialitat.
![[Pasted image 20261016161142.png]]

## LUKS 

**Preparació del disc per al xifratge LUKS**  
Per realitzar la prova de xifratge amb LUKS s'ha utilitzat una màquina virtual Ubuntu Desktop 26.04 executada amb VirtualBox. S'ha afegit un segon disc dur virtual independent del disc principal del sistema operatiu. D'aquesta manera es pot realitzar el procés de xifratge sense afectar la instal·lació d'Ubuntu.
![[Pasted image 20261016162542.png]]

**Identificació del disc virtual**  
Mitjançant la comanda `lsblk` s'han identificat els dispositius d'emmagatzematge disponibles. El disc `/dev/sda`, de 25 GB, correspon al disc principal on està instal·lat Ubuntu, mentre que `/dev/sdb`, de 5 GB, correspon al disc virtual afegit específicament per realitzar la prova de xifratge. Per tant, el procés amb LUKS es realitzarà exclusivament sobre `/dev/sdb`.![[Pasted image 20261016163023.png]]

**Inicialització del disc amb LUKS**  
S'ha utilitzat `cryptsetup` per inicialitzar el disc virtual `/dev/sdb` mitjançant LUKS. En executar `sudo cryptsetup luksFormat /dev/sdb`, el sistema ha advertit que l'operació sobrescriuria les dades existents i ha requerit una confirmació explícita. Posteriorment s'ha establert una frase de pas que serà necessària per desbloquejar el volum xifrat.
![[Pasted image 20261016164012.png]]

**Verificació del xifratge LUKS**  
Després d'inicialitzar el disc, s'ha executat `lsblk`, comprovant que `/dev/sdb` apareix identificat com a `crypto_LUKS`. Posteriorment, mitjançant `cryptsetup luksDump /dev/sdb`, s'ha verificat la configuració del volum. El resultat confirma que s'utilitza LUKS versió 2 i que existeix un `Keyslot` actiu associat a la frase de pas configurada. Per tant, el disc virtual ha quedat correctament preparat com a volum xifrat LUKS.
![[Pasted image 20261016164137.png]]
![[Pasted image 20261016164157.png]]

**Desbloqueig del volum LUKS**  
S'ha desbloquejat el volum `/dev/sdb` mitjançant la comanda `sudo cryptsetup open /dev/sdb disc_luks`. El sistema ha sol·licitat la frase de pas configurada anteriorment i, després d'introduir-la correctament, ha creat el dispositiu virtual `/dev/mapper/disc_luks`. Mitjançant `lsblk` s'ha comprovat que aquest dispositiu apareix sota `/dev/sdb` amb el tipus `crypt`, demostrant que el volum LUKS s'ha desbloquejat correctament.
![[Pasted image 20261016164255.png]]

**Creació del sistema de fitxers**  
Una vegada desbloquejat el volum LUKS, s'ha creat un sistema de fitxers `ext4` sobre el dispositiu `/dev/mapper/disc_luks` mitjançant `mkfs.ext4`. Posteriorment, amb `lsblk`, s'ha comprovat que `/dev/sdb` continua identificat com a `crypto_LUKS`, mentre que el volum desbloquejat `disc_luks` conté el sistema de fitxers `ext4`. D'aquesta manera, les dades que s'emmagatzemin al sistema de fitxers quedaran situades dins del volum protegit per LUKS.
![[Pasted image 20261016164345.png]]

**Muntatge i emmagatzematge de dades al volum xifrat**  
S'ha muntat el dispositiu desbloquejat `/dev/mapper/disc_luks` al directori `/mnt/disc_luks`. A continuació s'ha creat el fitxer `document_confidencial.txt` amb informació de prova. Mitjançant la comanda `cat` s'ha comprovat que, mentre el volum LUKS està desbloquejat i muntat, les dades es poden llegir amb normalitat. Finalment, `lsblk` confirma que el sistema de fitxers `ext4` es troba muntat a `/mnt/disc_luks`.
![[Pasted image 20261016164452.png]]

**Desmuntatge i bloqueig del volum LUKS**  
Per comprovar la protecció de les dades, primer s'ha desmuntat el sistema de fitxers amb `umount` i posteriorment s'ha tancat el volum mitjançant `cryptsetup close disc_luks`. En executar novament `lsblk`, el dispositiu desbloquejat `disc_luks` ha desaparegut i únicament es mostra `/dev/sdb` com a `crypto_LUKS`.

En intentar accedir posteriorment a `document_confidencial.txt`, el sistema indica que el fitxer no existeix a la ruta de muntatge. Això és degut al fet que el sistema de fitxers que el conté ja no està accessible: les dades continuen emmagatzemades al disc, però el volum LUKS està bloquejat.
![[Pasted image 20261016164607.png]]
![[Pasted image 20261016164619.png]]

**Desbloqueig i recuperació de les dades**

Finalment, s'ha tornat a desbloquejar `/dev/sdb` mitjançant `cryptsetup open`, introduint la frase de pas configurada. Després de muntar novament `/dev/mapper/disc_luks` a `/mnt/disc_luks`, s'ha pogut llegir correctament `document_confidencial.txt`.

Aquesta prova demostra que les dades no s'havien eliminat durant el bloqueig, sinó que havien quedat inaccessibles mentre el volum LUKS estava tancat. Només després d'introduir la frase de pas correcta s'ha pogut tornar a accedir al sistema de fitxers i recuperar-ne el contingut.

**Conclusió:** LUKS permet protegir un dispositiu d'emmagatzematge complet mitjançant xifratge. Quan el volum està bloquejat, el sistema de fitxers i les dades no són accessibles de manera normal; en proporcionar la credencial correcta, el volum es desbloqueja i les dades tornen a estar disponibles.
![[Pasted image 20261016164811.png]]

Integritat — Windows amb SFC

**Comprovació de la integritat dels fitxers de Windows amb SFC**

S'ha executat l'eina **System File Checker (SFC)** des d'un símbol del sistema amb privilegis d'administrador mitjançant la comanda:

`sfc /scannow`

L'anàlisi ha arribat correctament al 100 % i Windows ha retornat el missatge _“Protección de recursos de Windows no encontró ninguna infracción de integridad”_. Aquest resultat indica que SFC no ha detectat fitxers protegits del sistema corruptes, modificats o amb problemes d'integritat que requereixin reparació.

**Planificació d'actuació:** En no haver-se detectat cap infracció d'integritat, no és necessari efectuar cap acció correctiva immediata. Com a mesura preventiva, es recomana mantenir Windows actualitzat i repetir l'anàlisi amb SFC en cas de detectar comportaments anòmals, errors del sistema o sospites de corrupció dels fitxers. Si en una futura anàlisi SFC detectés fitxers corruptes que no pogués reparar, caldria analitzar els registres corresponents i aplicar eines de reparació addicionals abans de repetir la comprovació.

![[Pasted image 20261016171408.png]]

## GNU/Linux amb Rootkit Hunter

**Instal·lació de Rootkit Hunter**  
S'ha instal·lat l'eina `rkhunter` al sistema Ubuntu mitjançant el gestor de paquets APT. Posteriorment, s'ha executat `rkhunter --version` per verificar-ne la instal·lació, obtenint com a resultat **Rootkit Hunter 1.4.6**. Aquesta eina permet realitzar diferents comprovacions de seguretat del sistema, entre elles la detecció d'indicis associats a rootkits i altres anomalies.
![[Pasted image 20261016172250.png]]

**Incidència durant l'actualització de Rootkit Hunter:** En intentar executar `sudo rkhunter --update`, l'eina ha retornat un error relacionat amb l'opció `WEB_CMD`. S'ha comprovat la configuració de `rkhunter` i s'ha mantingut la configuració proporcionada pel paquet de la distribució. Com que l'actualització prèvia és una recomanació de l'activitat i no impedeix efectuar les comprovacions locals, s'ha continuat amb l'anàlisi del sistema utilitzant la versió instal·lada mitjançant els repositoris d'Ubuntu.

**Anàlisi del sistema GNU/Linux amb Rootkit Hunter**

S'ha executat `sudo rkhunter --check` sobre el sistema Ubuntu. L'eina ha comprovat propietats de fitxers, indicis de rootkits coneguts, processos, configuració de xarxa, comptes d'usuari i diferents elements del sistema.

L'anàlisi ha comprovat **144 fitxers** i **498 rootkits**, mostrant inicialment un fitxer sospitós i un possible rootkit. Tanmateix, aquests valors no constitueixen per si mateixos una confirmació d'infecció, sinó que indiquen que existeixen avisos que requereixen una revisió posterior. Texto pegado

La revisió dels resultats mostra que els rootkits coneguts comprovats no han estat detectats. Tampoc s'han trobat processos executant fitxers sospitosos, backdoors d'inici de sessió, directoris sospitosos ni indicis d'una backdoor d'Apache. Texto pegado

També s'han comprovat els ports de xarxa i les interfícies. No s'han detectat ports associats a backdoors ni interfícies funcionant en mode promiscu. Texto pegado

**Anàlisi dels warnings:** `rkhunter` ha marcat `/usr/bin/lwp-request` perquè es tracta d'un script Perl; aquest fet requereix comprovació, però no demostra per si mateix que el fitxer sigui maliciós. També s'ha detectat un segment de memòria compartida de 32 MB associat al procés `mutter-x11-frames`, superior al llindar d'1 MB configurat a `rkhunter`.

S'han detectat canvis als fitxers de comptes i grups perquè apareixen l'usuari `postfix` i els grups `postfix` i `postdrop`. En aquest cas els canvis són justificables, ja que Postfix s'ha instal·lat durant la preparació de les eines de la pràctica.

A `/dev/shm` s'han detectat dos fitxers amb noms `PostgreSQL.*`, relacionats amb l'ús de memòria compartida. També s'han marcat com a ocults `/etc/.java`, `/etc/.resolv.conf.systemd-resolved.bak` i `/etc/.updated`. L'existència d'un fitxer ocult no implica necessàriament que sigui malware, de manera que aquests elements s'han de revisar abans de considerar-los una amenaça.

Finalment, `rkhunter` ha advertit que l'opció SSH `PermitRootLogin` no està definida explícitament. Es tracta d'un avís de configuració de seguretat que convé revisar, però tampoc constitueix una detecció de rootkit. Els warnings d'aquest tipus expliquen per què el resum final informa de possibles elements sospitosos malgrat que les comprovacions específiques de rootkits coneguts hagin resultat negatives.
![[Pasted image 20261016173028.png]]

### Planificació d'actuació

> Davant dels avisos obtinguts, primer s'ha de verificar individualment cada element abans d'eliminar o modificar cap fitxer. Es comprovaria que `/usr/bin/lwp-request` pertany a un paquet legítim instal·lat, s'identificarien els processos responsables de la memòria compartida i es confirmaria que els elements `PostgreSQL.*` corresponen a processos legítims. També es revisarien els fitxers ocults detectats i la configuració del servei SSH.
> 
> Si algun element no pogués justificar-se mitjançant paquets, processos o configuracions legítimes, s'hauria d'aprofundir en la investigació abans de considerar el sistema segur. En canvi, els avisos justificats es documentarien com a possibles falsos positius. També es recomana mantenir el sistema actualitzat i repetir periòdicament les comprovacions d'integritat.

## VeraCrypt

**Prova del carregador de prearrencada:** Després de configurar el xifratge del sistema, VeraCrypt ha instal·lat el seu carregador de prearrencada. En reiniciar la màquina virtual, abans de carregar Windows, s'ha sol·licitat la contrasenya configurada a VeraCrypt. D'aquesta manera, l'accés al sistema operatiu queda condicionat a una autenticació prèvia.
![[Pasted image 20261016174705.png]]

**Verificació prèvia del xifratge:** Després de reiniciar el sistema i autenticar-se correctament mitjançant el carregador de prearrencada de VeraCrypt, Windows s'ha iniciat amb normalitat. En tornar a executar-se l'assistent, VeraCrypt ha informat que la prova del xifratge del sistema s'havia completat amb èxit. Això confirma que el mecanisme de prearrencada funciona correctament abans d'iniciar el xifratge definitiu.
![[Pasted image 20261016174922.png]]
![[Pasted image 20261016183830.png]]
