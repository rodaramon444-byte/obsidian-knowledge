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

Primerament, el que jo habia pensat es  