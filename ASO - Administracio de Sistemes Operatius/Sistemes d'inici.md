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
- **Kernel** -> gestiona processos
- **Aplicació** -> programa interactua usuari i executa 1r pla
- **Servei** -> programa associat SO i 2n pla
- **Procés** -> f(x) intern del SO
    - _Nota:_ Aplicacions i serveis -> generen processos (sincronitzar i planificar)
## 1. SystemV vs Upstart vs Systemd

[](https://github.com/mamadoucirebarry/asno#1-systemv-vs-upstart-vs-systemd)

## 1.1 Nivells d'execució (tasca systemd)

[](https://github.com/mamadoucirebarry/asno#11-nivells-dexecuci%C3%B3-tasca-systemd)

Crear un cire.target amb el meu nom i canviar a que sigui el per defecte.

- Que cride un .service que executara una terminal abans que s'executi res.
- Amb permisos root.