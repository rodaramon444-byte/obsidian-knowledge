![[Pasted image 20261002123500.jpg|252]]                                                       **Ramon Roda Adame** 

Tambe podreu troba la practica al meu Git com:  https://rodaramon444-byte.github.io/obsidian-knowledge/seguretat-en-sistemes/mpc037_ra1_pt1-dispositius-iot

**Exercici 1 (50%)**

Respon a les qüestions dels diferents apartats segons el que et demani l’enunciat de cada apartat en particular. 

1. El següent exercici consisteix en realitzar una cerca a Internet, per tal de veure que a la vida quotidiana estem envoltats de dispositius IoT. Prova de buscar informació sobre quins són els més utilitzats o estan més presents. Pots fer una taula indicant-ho en percentatges per exemple. Anota la font d’on has extret la informació.(Webgrafia) 
---

- Actualment, els dispositius IoT (Internet of Things) estan molt presents en la nostra vida quotidiana. Són dispositius capaços de connectar-se a Internet i intercanviar informació amb altres dispositius o serveis. 

- Alguns exemples habituals són els televisors intel·ligents, rellotges intel·ligents, altaveus com Alexa o Google Home, electrodomèstics intel·ligents, càmeres de seguretat, vehicles connectats o dispositius relacionats amb la salut. 

- Segons les dades publicades per Eurostat sobre l'ús de dispositius connectats a Internet a la Unió Europea durant l'any 2024, un **70,9 % de les persones d'entre 16 i 74 anys utilitzaven algun dispositiu IoT**. Els dispositius més utilitzats van ser els següents:

| Tipus de dispositiu IoT                        | Percentatge d'ús |
| ---------------------------------------------- | ---------------- |
| Televisors connectats a Internet (Smart TV)    | 57,9 %           |
| Rellotges intel·ligents i polseres d'activitat | 29,9 %           |
| Consoles de videojocs connectades              | 19,5 %           |
| Sistemes d'àudio domèstics connectats          | 19,3 %           |
| Altaveus intel·ligents / assistents virtuals   | 16,0 %           |
| Sistemes intel·ligents de gestió d'energia     | 14,2 %           |
| Electrodomèstics intel·ligents                 | 12,8 %           |
| Sistemes intel·ligents de seguretat de la llar | 11,8 %           |
| Vehicles amb connexió sense fils integrada     | 10,5 %           |
| Dispositius IoT relacionats amb la salut       | 7,9 %            |
| Joguines connectades a Internet                | 2,3 %            |
### Webgrafia

**Eurostat – Internet-connected devices are widely used in the EU**  
[https://ec.europa.eu/eurostat/en/web/products-eurostat-news/w/ddn-20250828-2](https://ec.europa.eu/eurostat/en/web/products-eurostat-news/w/ddn-20250828-2?utm_source=chatgpt.com)

**Eurostat – Digitalisation in Europe 2025**  
[https://ec.europa.eu/eurostat/web/interactive-publications/digitalisation-2025](https://ec.europa.eu/eurostat/web/interactive-publications/digitalisation-2025?utm_source=chatgpt.com)

## A continuació respon a les següents preguntes de forma breu i concisa.

---
### 1. En la recerca realitzada, quin tipus de dispositiu IoT t'ha resultat interessant o t'ha cridat l'atenció?

- Els dispositius IoT que més m'han cridat l'atenció són els **sistemes de seguretat intel·ligents**, com ara càmeres IP, timbres intel·ligents, sensors de moviment i alarmes.

- Em semblen interessants perquè permeten controlar la seguretat d'una casa o empresa des de qualsevol lloc mitjançant Internet. Per exemple, podem veure les càmeres des del telèfon mòbil o rebre una notificació quan un sensor detecta moviment.

### 2. Des del punt de vista de la seguretat, com creus que aquest tipus de dispositius ens poden afectar?

- Els dispositius IoT poden suposar un risc de seguretat si no estan correctament configurats o actualitzats.

- Una vulnerabilitat podria permetre que un atacant accedís al dispositiu, obtingués informació privada o fins i tot el controlés remotament. Per exemple, una càmera IP vulnerable podria permetre veure imatges de l'interior d'una casa.

- També existeix el risc que aquests dispositius siguin infectats amb malware i utilitzats per formar part d'una botnet.

- Per reduir aquests riscos és important **canviar les contrasenyes predeterminades, mantenir el firmware actualitzat, utilitzar contrasenyes segures i evitar exposar directament els dispositius a Internet**.

# 2. Realitza una cerca dels diferents sistemes operatius existents per a                       dispositius IoT. Fes una taula, indicant els requeriments hardware d’aquests.

- Existeixen diferents sistemes operatius especialment dissenyats per funcionar en dispositius IoT. Alguns estan destinats a dispositius molt petits, com sensors o microcontroladors, mentre que altres poden funcionar en dispositius més potents, com Raspberry Pi, gateways IoT o ordinadors industrials.

- A continuació es mostren alguns dels sistemes operatius IoT més coneguts i els seus requeriments aproximats:

|Sistema operatiu|RAM / requeriments|Emmagatzematge / Flash|Arquitectures / Hardware|
|---|---|---|---|
|**Ubuntu Core**|Mínim 512 MB RAM|Mínim 1 GB|amd64, arm64, armhf i riscv64|
|**FreeRTOS**|Depèn del microcontrolador i configuració. En implementacions IoT completes pot treballar amb aproximadament 64 KB RAM|Aproximadament 256 KB Flash en determinades configuracions amb OTA|Microcontroladors ARM, RISC-V i altres|
|**Zephyr OS**|Molt reduïda i variable segons la configuració|Una configuració mínima pot tenir un footprint de ROM d'aproximadament 7-8 KB|ARM, x86, RISC-V i altres|
|**RIOT OS**|Depèn de la placa. Existeixen dispositius compatibles amb aproximadament 20 KB RAM|Alguns dispositius compatibles disposen de 128 KB Flash|ARM, ESP32, RISC-V i altres microcontroladors|
|**Contiki-NG**|Pot funcionar, per exemple, en plataformes amb 32 KB RAM|Plataformes compatibles com OpenMote disposen de 256/512 KB Flash|Principalment microcontroladors de baix consum|
	
- Els requeriments de FreeRTOS, Zephyr, RIOT i Contiki-NG **no són un mínim universal**, perquè depenen de la placa, els controladors i les funcionalitats que s'incloguin en cada compilació. Són sistemes molt configurables.
	
- Ubuntu Core, en canvi, té uns requisits generals oficials de **512 MB de RAM i 1 GB d'emmagatzematge**, i suporta arquitectures com AMD64, ARM i RISC-V.
	
- Zephyr està especialment pensat per ocupar molt poc espai. La seva documentació mostra configuracions mínimes que poden tenir un footprint de ROM d'aproximadament **7-8 KB**, tot i que una aplicació IoT real necessitarà més memòria en funció dels serveis utilitzats.
	
- RIOT també està orientat a dispositius amb recursos molt limitats. Els requeriments depenen de l'aplicació i del hardware utilitzat; per exemple, RIOT suporta microcontroladors CC26x0/CC13x0 amb **20 KB de RAM i 128 KB de Flash**.
	
- Contiki-NG pot funcionar sobre dispositius molt limitats. Per exemple, la plataforma OpenMote CC2538 compatible amb Contiki-NG disposa de **32 KB de RAM i 256/512 KB de Flash**.

### Webgrafia

**Ubuntu Core – System requirements**  
[https://documentation.ubuntu.com/core/system-requirements/](https://documentation.ubuntu.com/core/system-requirements/?utm_source=chatgpt.com)

**FreeRTOS**  
[https://www.freertos.org/](https://www.freertos.org/?utm_source=chatgpt.com)

**Zephyr Project**  
[https://www.zephyrproject.org/](https://www.zephyrproject.org/?utm_source=chatgpt.com)

**RIOT OS**  
[https://www.riot-os.org/](https://www.riot-os.org/?utm_source=chatgpt.com)

**Contiki-NG**  
[https://contiki-ng.org/](https://contiki-ng.org/?utm_source=chatgpt.com)

##  A continuació respon a les següents preguntes de forma breu i concisa.

### 1. Coneixies o havies sentit a parlar d'algun dels sistemes operatius trobats? Què et semblen els requeriments hardware que tenen per tal de poder ser instal·lats i executats?

- Coneixia **Ubuntu**, però no sabia que existia una versió anomenada **Ubuntu Core** especialment orientada a dispositius IoT.

- El que més m'ha cridat l'atenció són els pocs recursos que necessiten alguns d'aquests sistemes operatius. Sistemes com Zephyr, RIOT o Contiki-NG poden funcionar en microcontroladors amb molt poca memòria RAM i emmagatzematge.

- Això és important en IoT perquè molts dispositius, com sensors, actuadors o sistemes domòtics, tenen un hardware molt més limitat que un ordinador convencional.

### 2. Busca articles per Internet que parli sobre problemes de seguretat que van aparèixer i que van afectar a diferents dispositius IoT. Hauràs d’anotar els links, i explicar de forma breu, clara i concisa el problema que van tenir i les seves conseqüències. (Després ho hauràs de comentar a classe amb la resta de companys).

- Un dels casos de seguretat relacionats amb IoT més coneguts és la **botnet Mirai**.

#### Botnet Mirai – 2016

Mirai era un malware que buscava automàticament dispositius IoT accessibles des d'Internet que tenien una seguretat deficient, especialment dispositius que mantenien noms d'usuari i contrasenyes predeterminats o incorporats pel fabricant.

Quan trobava un dispositiu vulnerable, l'infectava i aquest passava a formar part d'una **botnet**, és a dir, una xarxa formada per molts dispositius infectats que podien ser controlats remotament.

L'octubre de 2016, la botnet Mirai va ser utilitzada per realitzar un atac distribuït de denegació de servei (**DDoS**) contra l'empresa Dyn, que proporcionava serveis DNS.

L'atac va provocar problemes d'accés a importants serveis i pàgines d'Internet. Aquest cas va demostrar que dispositius IoT aparentment simples poden convertir-se en una amenaça important si no disposen de mesures de seguretat adequades.

**Conseqüències principals:**

- Gran quantitat de dispositius IoT infectats.
    
- Utilització dels dispositius sense que els seus propietaris ho sabessin.
    
- Creació d'una gran botnet.
    
- Atacs DDoS contra serveis d'Internet.
    
- Interrupció temporal de l'accés a diferents pàgines i serveis.
    
- Va demostrar la importància de canviar les credencials predeterminades i mantenir actualitzats els dispositius IoT.
    

### Font

**CISA – Informe que analitza l'atac de Mirai contra Dyn**  
[CISA – Mirai Botnet i atac contra Dyn](https://www.cisa.gov/sites/default/files/publications/NSTAC%20Report%20to%20the%20President%20on%20ICR%20FINAL%20%2810-12-17%29%20%281%29-%20508%20compliant_0.pdf?utm_source=chatgpt.com)

---

**Exercici 2 (50%)

1. Un cop realitzat l’exercici 1, hauràs vist que hi ha molts sistemes operatius per dispositius IoT. Es pretén que en aquest exercici realitzis la instal·lació d’un sistema operatiu per IoT en una màquina virtual. Hauràs de realitzar les captures de pantalla amb tots els passos en el procés d’instal·lació realitzats, i comentar les mateixes. Abans de fer res, seria interessant que miressis per Internet i valoressis quin sistema operatiu instal·lar veient les possibilitats d’aquest. (Raspbian, Windows 10 IoT, ...)
    

Per finalitzar amb l’exercici, analitzaràs les diferents possibilitats del sistema operatiu instal·lat, així com els menús que conté el sistema operatiu en qüestió un cop instal·lat, indicant clarament sobre quin altre sistema operatiu es basa, i què et permet fer el sistema operatiu (aplicacions que conté, serveis, ...)**

---
## REALITZACIÓ

- He escollit **Ubuntu Core** perquè està orientat específicament a **IoT i sistemes embeguts/edge**, utilitza paquets `snap`, té actualitzacions transaccionals i està dissenyat amb un enfocament important en la seguretat.
- Abans de crear la màquina virtual necessitem descarregar la imatge d’**Ubuntu Core** adequada per executar-la virtualitzada.

- **Comprovació de l’entorn de virtualització.** El sistema amfitrió utilitzat és Ubuntu 24.04.4 LTS i s’ha comprovat que VirtualBox està instal·lat correctament, concretament la versió 7.1.18.
	![[Pasted image 20261002115030.png]]

- **Versions disponibles d’Ubuntu Core.** S’ha instal·lat Multipass i s’ha utilitzat l’ordre `multipass find` per consultar les imatges disponibles. Entre les diferents versions s’ha escollit **Ubuntu Core 26**, ja que és la versió més recent disponible d’Ubuntu Core.
	![[Pasted image 20261002120112.png|640]]

- **Creació de la màquina virtual IoT.** S’ha creat i iniciat una màquina virtual anomenada `iot-core` utilitzant la imatge d’Ubuntu Core 26 mitjançant Multipass. El missatge `Launched: iot-core` confirma que el procés s’ha completat correctament.
	![[Pasted image 20261002120130.png]]

- **Comprovació de l’estat de la màquina virtual.** Mitjançant les ordres `multipass list` i `multipass info iot-core` s’ha comprovat que la màquina virtual està en execució. Ubuntu Core 26 disposa d’1 CPU, aproximadament 1 GB de memòria RAM i 8,9 GB d’espai en disc. També se li ha assignat l’adreça IP `10.189.183.80`.
	![[Pasted image 20261002120309.png]]

- **Accés a Ubuntu Core 26 i identificació del sistema.** S’ha accedit a la màquina virtual mitjançant `multipass shell iot-core`. Amb `cat /etc/os-release` s’ha comprovat que el sistema instal·lat és Ubuntu Core 26. També s’ha utilitzat `uname -a` per consultar el kernel, observant que utilitza Linux 7.0.0-38-generic sobre arquitectura x86_64.
	![[Pasted image 20261002120457.png]]
	
	![[Pasted image 20261002120507.png]]

- **Aplicacions i serveis d’Ubuntu Core 26.** Mitjançant `snap list` s’han consultat els components instal·lats en format Snap. Entre aquests es troben `core26`, que proporciona el sistema base; `pc-kernel`, que proporciona el kernel; `pc`, relacionat amb l’arrencada i el maquinari; `snapd`, encarregat de gestionar els paquets Snap; i `console-conf`, utilitzat per a la configuració inicial. També s’han consultat els serveis en execució amb `systemctl`, observant serveis com SSH, gestió de xarxa, resolució DNS i Snap.
	![[Pasted image 20261002121430.png]]

	![[Pasted image 20261002121439.png|608]]

- **Instal·lació d’un servei IoT en Ubuntu Core.** S’ha instal·lat Mosquitto 2.1.2 mitjançant el sistema de paquets Snap. Mosquitto és un broker MQTT que permet l’intercanvi de missatges entre dispositius IoT. Amb `snap services` s’ha comprovat que el servei està habilitat i actiu.
	![[Pasted image 20261002121701.png]]

- **Prova de comunicació MQTT.** Per comprovar el funcionament del broker Mosquitto s’han utilitzat dues terminals. En una s’ha creat un subscriptor al tòpic `asix/iot` mitjançant `mosquitto_sub`, mentre que des de l’altra s’ha publicat el missatge “Hola IoT des d’Ubuntu Core” mitjançant `mosquitto_pub`. El missatge s’ha rebut correctament, demostrant el funcionament del protocol MQTT i del broker instal·lat.
	![[Pasted image 20261002121950.png]]

	![[Pasted image 20261002122230.png|640]]

- **Comprovació de la configuració del sistema.** S’ha analitzat la configuració de xarxa, la versió del gestor Snap i l’emmagatzematge d’Ubuntu Core. La màquina disposa de la interfície `ens3` amb l’adreça IPv4 `10.189.183.80/24` i utilitza `10.189.183.1` com a porta d’enllaç. El sistema funciona sobre arquitectura AMD64 amb el kernel Linux `7.0.0-38-generic` i utilitza Snap 2.75.2 per gestionar els components i aplicacions.
	![[Pasted image 20261002122726.png]]
	
	![[Pasted image 20261002122738.png]]