## 1. Confidencialitat – EFS

**Creació de l'usuari de proves**  
S'ha creat l'usuari local `usuari_efs` mitjançant la comanda `net user`. Posteriorment s'ha comprovat la seva configuració i s'ha verificat que pertany únicament al grup local **Usuarios**, per tant, no disposa de privilegis d'administrador. Aquest compte serà l'utilitzat per realitzar el xifratge EFS.
![[Pasted image 20261016154223.png]]