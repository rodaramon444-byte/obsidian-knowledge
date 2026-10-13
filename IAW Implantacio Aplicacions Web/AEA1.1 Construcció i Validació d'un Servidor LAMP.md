## Fase 1: Preparació de l'Entorn Virtual (El Sistema Operatiu 'L')

### 1. Creació de la Màquina Virtual (MV)

- **Creació de la Màquina Virtual (MV):**  
Configura una nova MV a VirtualBox amb uns requisits mínims de **2 GB de RAM i 25 GB de disc**. Assegura't que la configuració de xarxa estigui en mode pont (**Bridged Adapter**) per obtenir una IP a la xarxa local.

![[Pasted image 20261013192818.png|460]]

### 2. Instal·lació d'Ubuntu Server

Realitza la instal·lació estàndard d'Ubuntu Server. Durant el procés, és crucial que seleccionis l'opció per instal·lar el servidor OpenSSH, ja que et permetrà administrar la màquina de forma remota.
![[Pasted image 20261013193526.png]]

### 3. Connexió Remota

**Enunciat:**

Un cop finalitzada la instal·lació, inicia la MV. Troba l'adreça IP que se li ha assignat a la teva xarxa. Des de la teva màquina amfitriona, estableix una connexió remota per SSH. A partir d'aquest punt, tota l'administració s'ha de fer a través d'aquesta sessió remota.

# Fase 2: Implementació del Servidor Web (La 'A' d'Apache)

## 1. Instal·lació

### Enunciat

Utilitzant el gestor de paquets `apt`, busca i instal·la el paquet corresponent al servidor web Apache2. Recorda actualitzar els repositoris abans de res.
![[Pasted image 20261013193730.png]]

# Fase 2: Implementació del Servidor Web (La 'A' d'Apache)

## 2. Verificació del Servei

### Enunciat

Comprova que el servei d'Apache s'està executant correctament. Fes servir l'eina de gestió de serveis del sistema per consultar el seu estat.

**Prova de funcionament:** Accedeix a l'adreça IP de la teva MV des d'un navegador web. Si tot és correcte, hauries de veure la pàgina de benvinguda per defecte d'Apache a Ubuntu.

![[Pasted image 20261013193858.png]]
![[Pasted image 20261013193948.png|522]]

# Fase 3: Implementació de la Base de Dades (La 'M' de MariaDB)

## 1. Instal·lació

### Enunciat

Instal·la el paquet del servidor MariaDB utilitzant el gestor de paquets.
![[Pasted image 20261013194429.png]]

2. ## Securització (Pas Crític)

MariaDB inclou un script interactiu per millorar la seguretat de la instal·lació inicial. Executa aquest script i segueix les seves recomanacions per establir una contrasenya de root, eliminar usuaris anònims i altres mesures de seguretat.

- He executat l'eina `mariadb-secure-installation` per millorar la seguretat de la instal·lació. Durant el procés he eliminat els usuaris anònims, deshabilitat l'accés remot de l'usuari root, eliminat la base de dades de proves i recarregat les taules de privilegis perquè els canvis s'apliquin immediatament.
## 3. Creació de la BBDD per al CMS

Accedeix al client de línia de comandes de MariaDB i executa les sentències SQL necessàries per crear una base de dades anomenada `cms_db`, crear un usuari `cms_user` i assignar-li tots els permisos sobre `cms_db`. Finalment, s'ha de fer una captura de terminal que demostri que la base de dades i l'usuari s'han creat correctament.