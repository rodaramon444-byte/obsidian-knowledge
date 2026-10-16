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