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