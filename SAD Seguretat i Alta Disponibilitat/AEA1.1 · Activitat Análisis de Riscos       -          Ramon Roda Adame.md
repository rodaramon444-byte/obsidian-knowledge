![[Pasted image 20261015152559.png]]


**1. Identificació dels actius**

En primer lloc, s’identifiquen els diferents actius que formen part del sistema informàtic de l’IES de l’Ebre. Aquests actius seran la base sobre la qual es realitzarà posteriorment l’anàlisi de riscos, identificant les amenaces que els poden afectar i les mesures que es poden aplicar per reduir aquests riscos.

**2. Preparació de les eines per a l’anàlisi de riscos**
A continuació es realitza l’anàlisi de riscos dels principals actius del sistema informàtic de l’IES de l’Ebre. Per a cada actiu s’identifiquen les amenaces que el poden afectar i es valora la probabilitat que es produeixin, l’impacte que tindrien i el nivell de risc resultant.

|ID|Actiu|Amenaça|Probabilitat|Impacte|Risc|
|---|---|---|---|---|---|
|R01|Ordinadors de Direcció|Accés no autoritzat|Mitjana|Alt|Alt|
|R02|Ordinadors de Direcció|Malware / programari maliciós|Alta|Alt|Alt|
|R03|Disc dur en xarxa|Avaria del disc|Mitjana|Molt alt|Alt|
|R04|Disc dur en xarxa|Accés no autoritzat|Baixa|Molt alt|Mitjà|
|R05|Servidor Moodle|Caiguda del servidor|Mitjana|Alt|Alt|
|R06|Servidor Moodle|Atac informàtic|Mitjana|Alt|Alt|
|R07|Google Drive|Robatori de credencials|Mitjana|Alt|Alt|
|R08|Google Drive|Eliminació accidental de fitxers|Mitjana|Mitjà|Mitjà|
|R09|Xarxa Wi-Fi|Accés no autoritzat|Alta|Mitjà|Alt|
|R10|Xarxa Wi-Fi|Intercepció de comunicacions|Mitjana|Alt|Alt|
|R11|Esfer@|Robatori de credencials|Baixa|Molt alt|Alt|
|R12|Esfer@|Indisponibilitat del servei|Baixa|Alt|Mitjà|
|R13|Còpies de seguretat|Pèrdua o corrupció de la còpia|Baixa|Molt alt|Alt|
|R14|Còpies de seguretat|Còpia de seguretat no actualitzada|Mitjana|Alt|Alt|

---

**3. Identificació i selecció d’amenaces**

Seguint el catàleg d’amenaces, s’han identificat les possibles amenaces que poden afectar els actius del sistema informàtic del centre. S’han seleccionat com a mínim dues amenaces de cadascuna de les quatre categories del catàleg.

|Secció|Codi|Amenaça|Actiu principal afectat|
|---|---|---|---|
|**Desastres naturals [N]**|N.1|Foc / incendi|Servidors i equips informàtics|
|**Desastres naturals [N]**|N.2|Danys per aigua|Servidors i equips informàtics|
|**Origen industrial [I]**|I.5|Avaria d’origen físic o lògic|Disc de xarxa / servidor Moodle|
|**Origen industrial [I]**|I.6|Tall del subministrament elèctric|Servidors i xarxa|
|**Errors i fallades no intencionades [E]**|E.1|Errors dels usuaris|Ordinadors / Drive / dades|
|**Errors i fallades no intencionades [E]**|E.2|Errors de l’administrador|Servidors / xarxa / Moodle|
|**Atacs intencionats [A]**|A.5|Suplantació de la identitat de l’usuari|Drive / Esfer@|
|**Atacs intencionats [A]**|A.11|Accés no autoritzat|Direcció / Drive / Esfer@|

---

**4. Anàlisi i valoració dels riscos**

Una vegada identificades les amenaces, es realitza la seva valoració tenint en compte la probabilitat que es produeixin i l’impacte que tindrien sobre els actius del centre. A partir d’aquests dos factors s’estableix el nivell de risc i es determina quins riscos necessiten una major prioritat.

|ID|Secció|Amenaça|Actiu afectat|Probabilitat|Impacte|Nivell de risc|
|---|---|---|---|---|---|---|
|R01|Desastres naturals|Foc / incendi|Servidors i equips informàtics|Baixa|Molt alt|**Alt**|
|R02|Desastres naturals|Danys per aigua|Servidors i equips informàtics|Baixa|Alt|**Mitjà**|
|R03|Origen industrial|Avaria física o lògica|Disc de xarxa / Moodle|Mitjana|Molt alt|**Alt**|
|R04|Origen industrial|Tall del subministrament elèctric|Servidors i xarxa|Mitjana|Alt|**Alt**|
|R05|Errors no intencionats|Errors dels usuaris|Ordinadors / Drive / dades|Alta|Mitjà|**Alt**|
|R06|Errors no intencionats|Errors de l’administrador|Servidors / xarxa / Moodle|Mitjana|Alt|**Alt**|
|R07|Atacs intencionats|Suplantació d’identitat|Drive / Esfer@|Mitjana|Molt alt|**Alt**|
|R08|Atacs intencionats|Accés no autoritzat|Direcció / Drive / Esfer@|Mitjana|Molt alt|**Alt**|

---

**5. Registre, classificació i priorització d’iniciatives**

A partir dels riscos identificats, es proposen diferents iniciatives de seguretat amb l’objectiu de reduir la probabilitat que es produeixin les amenaces o minimitzar-ne l’impacte. Per a cada amenaça es proposen tres iniciatives i se’ls assigna una prioritat segons la seva importància.

|ID|Amenaça|Iniciativa proposada|Prioritat|
|---|---|---|---|
|I01|Foc / incendi|Mantenir la revisió periòdica dels extintors|Alta|
|I02|Foc / incendi|Instal·lar detectors de fum a les sales amb servidors|Alta|
|I03|Foc / incendi|Mantenir còpies de seguretat fora de la ubicació principal|Alta|
|I04|Danys per aigua|Evitar instal·lar equips informàtics prop de canonades o zones amb risc d’aigua|Mitjana|
|I05|Danys per aigua|Col·locar els servidors i equips crítics elevats respecte del terra|Mitjana|
|I06|Danys per aigua|Instal·lar sensors de detecció de fuites d’aigua|Mitjana|
|I07|Avaria física o lògica|Fer còpies de seguretat periòdiques|Alta|
|I08|Avaria física o lògica|Revisar periòdicament l’estat dels discs i servidors|Alta|
|I09|Avaria física o lògica|Disposar de sistemes redundants per a la informació crítica|Mitjana|
|I10|Tall del subministrament elèctric|Instal·lar sistemes SAI als servidors|Alta|
|I11|Tall del subministrament elèctric|Configurar l’apagada automàtica i segura dels servidors|Alta|
|I12|Tall del subministrament elèctric|Revisar periòdicament els SAI i les seves bateries|Mitjana|
|I13|Errors dels usuaris|Formar els usuaris en bones pràctiques de seguretat|Alta|
|I14|Errors dels usuaris|Limitar els permisos segons les necessitats de cada usuari|Alta|
|I15|Errors dels usuaris|Facilitar mecanismes de recuperació de fitxers eliminats accidentalment|Mitjana|
|I16|Errors de l’administrador|Documentar els canvis realitzats als sistemes|Alta|
|I17|Errors de l’administrador|Fer una còpia de la configuració abans de realitzar canvis importants|Alta|
|I18|Errors de l’administrador|Comprovar els canvis en un entorn de proves abans d’aplicar-los|Mitjana|
|I19|Suplantació d’identitat|Activar l’autenticació de doble factor (2FA)|Alta|
|I20|Suplantació d’identitat|Aplicar una política de contrasenyes robustes|Alta|
|I21|Suplantació d’identitat|Formar els usuaris per detectar intents de phishing|Alta|
|I22|Accés no autoritzat|Revisar periòdicament els permisos d’accés|Alta|
|I23|Accés no autoritzat|Bloquejar els comptes després de diversos intents d’accés fallits|Mitjana|
|I24|Accés no autoritzat|Registrar i supervisar els accessos als sistemes crítics|Alta|

---

**INCIDÈNCIES**

Durant la realització de la pràctica no s’han produït incidències destacables. S’ha pogut realitzar l’anàlisi de riscos a partir de la informació proporcionada sobre la infraestructura i els serveis informàtics del centre.

