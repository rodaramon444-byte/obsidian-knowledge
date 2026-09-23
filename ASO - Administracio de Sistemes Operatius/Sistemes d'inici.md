**1 SystemV vs Upstant vs Systemd**
- 1.1 Runlevel o Target?
- 1.2 Quin es el nostre SO?
**2 SystemV**
- 2.1 Directoris
- 2.2 Proces d'arrancada
**3 systemd**
- 3.1 Directoris
- 3.2 Systemctl
- 3.3 Depencencia
- 3.4 Modificar target provisionalment
- 3.5 Modificar target definitivament
- 3.6 Afegir/treure serveis target 
- 3.7 Crear nou target
- 3.8 Crear nou servei

**CONCEPTES**
- **Kernel** -> gestiona processos
- **Aplicació** -> programa interactua usuari i executa 1r pla
- **Servei** -> programa associat SO i 2n pla
- **Procés** -> f(x) intern del SO
    - _Nota:_ Aplicacions i serveis -> generen processos (sincronitzar i planificar)
---

## 1. SystemV vs Upstart vs Systemd

## 1.1 Nivells d'execució (tasca systemd)
---
- **Crear target propi, fer-lo default target i comprovar que accediu amb el vostre target.**
- **Crear un servei dintre del vostre target i comprovar que s’inicia correctament al reiniciar.**
- **Modificar el servei per a que execute un script amb permisos root.**
- **Programar script amb el que vulgueu i executar-lo manualment per a veure si funciona**

**Crear target propi, fer-lo default target i comprovar que accediu amb el vostre target.** 
- Primer he creat el ramon.target amb: `sudo nano /etc/systemd/system/ramon.target` 
- Tot seguit he posat:
	[Unit]
	Description=Target personalitzat de Ramon
	Requires=graphical.target
	After=graphical.target
	AllowIsolate=yes`

![[Pasted image 20260923170943.png]]

**Crear un servei dintre del target i comprovar que s'inicia en reiniciar**. +
- Primer he creat el ramon.service amb la comanda seguent:  `sudo nano /etc/systemd/system/ramon.service`
- De moment he fet un servei molt simple per comprovar que arrenca:
	[Unit]
	Description=Servei de Ramon
	After=graphical.target
	[Service]
	Type=oneshot
	ExecStart=/usr/bin/touch /tmp/ramon-servei-iniciat
	RemainAfterExit=yes
	[Install]
	WantedBy=ramon.target

![[Pasted image 20260923171510.png]]

Al reiniciar hem sortia aquest error, pero simplement he fet sudo systemctl reboot -i, ja que he fet una instantania de la mv
![[Pasted image 20260923172629.png]]

Al reiniciar he comprovat que el servei arranca automàticament i el fitxer demostrar que s'ha executat com a root:
![[Pasted image 20260923173029.png]]

Ara que ja tinc el target amb el servei i tot, vaig a crear el script
![[Pasted image 20260923173520.png]]

Despres li he donat permisos
![[Pasted image 20260923173615.png]]

Tot seguit he modificat l'arxiu el servei per a que execute el script:
![[Pasted image 20260923173745.png]]

I he actualitzat el systemd, i tambe he fet una prova de l'escript, per veure si funciona:
![[Pasted image 20260923174648.png]]

Ara he modificat l'escript d'aquesta manera
![[Pasted image 20260923175112.png]]

Tot seguit l'hi he donat permisos i he fet una prova, i ha obert automaticament telegram
![[Pasted image 20260923175241.png]]

Ara per el tema de la captura de la pantalla, he hagut de provar i investigar molt, ja que amb Ubuntu 26 no deixa canviar la interficie a X11, i amb wyland no garanteix que un procés root pugui saltar-se silenciosament aquests permisos, pero he pogut saltar-me el proces de demanar permis amb la comanda: `gdbus call --session \ --dest org.freedesktop.portal.Desktop \ --object-path /org/freedesktop/portal/desktop \--method org.freedesktop.portal.Screenshot.Screenshot \  "" \  "{'interactive':<false>}"` , 

Tot seguit el que he ingeniat es un programa amb python que:
- Sol·licitar una captura de pantalla al portal de Wayland. 
- Esperar automàticament la resposta del sistema.
- Obtenir la ubicació de la imatge generada.
- Guardar-la automàticament com:
`/home/ramon/Imatges/captura-telegram.png´
![[Pasted image 20260923190612.png]]

Ara he modificat ramon.sh per a que nomes amb aquell script funcione tot, despres l'he provat, i efectivament funciona i obre telegram i fa una captura de pantalla
![[Pasted image 20260923191422.png]]

![[Pasted image 20260923191530.png]]

![[Pasted image 20260923191604.png|545]]