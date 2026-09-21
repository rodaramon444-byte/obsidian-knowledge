**RA1 ELEMENTS DE LES BD**

Dades --> organitzades per fitxers
Fitxers--> diferents tipus i formats (txt...)
- Els pixels d'una imatge es guarden en "Vectors"

- El contingut es text o binari, binari es tot allo que es apart de text (imatges...).

Maneres de organitzar les dades (seqüencial, directa o indexada)
- Sequencial (Per arribar a una dada, primer has de pasar per les altres)
- Directa (Pots anar directe, no has de pasar per cap)
- Indexada (simulant un index)

Tambe es organitzaven per la seva utilitat, 
- mestres, que son els importants o fonamentals
- moviments, que son els que s'utilitzen per modificar els mestres
- historics, son els obsolets, que no son necessaris per a l'us diari

**Fitxers de text**
- ASCII: es una taula on s'assigna un valor numeric a cada caracter (256)
-  Extensions fitxer de text: .ini, .inf, .conf, .sql, .c, .java, .html, .php, .css, .xml, rtf, .ps, .tex
- Fitxers binaris: 
	- D'imatge: .jpg, .gif, .tiff, .bmp, .png i molts altres 
		- De vídeo: .mpg, .mov, .avi, .qt 
		- Comprimits o empaquetats: .zip, .Z, .gz, .tar, .lhz 
		-  Executables o compilats: .exe, .com, .cgi, .o, .a 
		- Processadors de text: .doc, .odt

**Bases de dades:**
	- Col·lecció d'informació que pertany a un mateix context (o problema), que està emmagatzemada de forma organitzada en fitxers.
- Una base de dades està organitzada en **taules** on guardem informació que fa referencia a un objecte **o** succes:
	- Objecte: Dades de un client o un producte
	- Succes: Una compra de un client

Les taules es relacionen formant vincles o relacions. A les taules les files son registres i les columnes son camps.

**DADA**: És un tros d'informació concreta sobre algun concepte o succés. Per exemple, 1996 és un número que representa un any de naixement. Les dades es caracteritzen per tenir un tipus.

**TIPUS DE DADA**: El tipus de dada indica la naturalesa del camp. Així, es pot tenir... 
	• Dades numèriques. Són aquelles amb les que podem realitzar càlculs aritmètics (sumes, restes,...) 
	• Dades alfanumèriques. Són aquelles que contenen caràcters alfabètics i dígits numèrics.

**CAMP**: Un camp és un identificador per a tota una família de dades. Cada camp pertany a un tipus de dades. Per exemple, el camp data_naixement representa les dates de naixement de les persones que tenim a la taula i pertany al tipus data. També se li pot dir columna.

**REGISTRE**: És una recol·lecció de dades referents a un mateix concepte o succés. Per exemple, les dades d'una persona poden ser el NIF, data_naixement, nom, direcció, ... . També se'ls hi pot dir tuples o files.

**CAMP CLAU**: És un camp especial que identifica de forma única a cada registre. Així el NIF que és únic per a cada persona, és un camp clau. Més endavant veurem que hi ha diversos tipus de camps clau.

**TAULA**: És un conjunt de registres agrupats baix un mateix nom i que els representa a tots. Exemple, tots els clients d'una BD s'emmagatzemen en una taula amb nom CLIENTS.

**CONSULTA**: És una instrucció per a fer peticions a una BD. Pot ser una cerca simple d'un registre específic o una sol·licitud per a seleccionar tots aquells registres que compleixin una sèrie de criteris

**Estructura d'una BD**
Una BD emmagatzema les dades mitjançant un **esquema**. 
	**Esquema**: És la definició de l'estructura on s'emmagatzemen les dades.
	![[Pasted image 20260921125247.png]]

**Evolució i tipus de bases de dades**
**Dècada del 1950** 
	- S'inventen les cintes magnètiques, que només podien ser llegides de forma seqüencial i ordenada. 
	 - Les cintes emmagatzemaven fitxers amb registres que es processaven seqüencialment junt amb fitxers de moviments per a generar nous fitxers actualitzats. 
	 - Aquests sistemes es coneixen com aplicacions basades en sistemes de fitxers i constitueixen la generació zero de les bases de dades. De fet en aquesta època no existia encara el concepte de BD.

**Dècada del 1960** 
	- Es generalitza l'ús de discos magnètics, on la seva característica principal es que es pot accedir de forma directa a qualsevol part dels fitxers que conté, sense haver de passar pels anteriors. 
	- Amb aquesta tecnologia apareixen les bases de dades jeràrquiques i en xarxa, les qual aprofiten la capacitat d'accés directe a la informació dels discos per a estructurar la informació en forma de llistes enllaçades i arbres d'informació.

	Notació històrica: A l'octubre de 1969 neix el primer model de base de dades en xarxa, conegut com CODASYL (Conference on Data System Language). Posteriorment va ser millorat per IBM mitjançant el model IMS (Information Management System) per al programa Apollo de la NASA.

**Dècada del 1970**
	Edgar Frank Codd, científic informàtic anglès de IBM, publica l'any 1970 en un article 'Un model relaciona de dades per a grans bancs de dades compartits', on defineix el model relacional, basat en la lògica de predicats i la teoria de conjunts. 
	• Neixen les bases de dades relacionals, o segona generació de bases de dades. 
	• Larry Ellison, fundador d'Oracle, s'inspira amb l'article de Codd per a desenvolupar el famós motor de base de dades, el qual va començar com un projecte per a la CIA. 
	• La potent base matemàtica d'aquest model és el secret del seu èxit. 
	• Avui en dia el model relacional de Codd, encara que existeixen moltes alternatives, segueix sent el més utilitzat a tots els nivells.

**Dècada del 1980** 
	• IBM llença el seu motor de bases de dades DB2. 
	• Anys més tard IBM crea el llenguatge SQL (Structured Query Language), un potent llenguatge de consultes per a manipular informació de les bases de dades relacionals. Dècada del 1990 
	• A mitjans anys 90 IBM treu al mercat una versió de DB2 que és capaç de dividir una base de dades gran en diversos servidors comunicats per línies de gran velocitat, creant d'aquesta manera BDs paral·leles. 
	• El nom que va rebre aquesta versió va ser DB2 Parallel Edition, ha anat evolucionant fins la versió DB2 Data Partition Feature, únic SGBD d'aquest tipus per a sistemes distribuïts. 
	• A finals de 1990 IBM i Oracle incorporen a les seves bases de dades la capacitat de manipular objectes, creant així les bases de dades orientades a objectes.

***
**Els Sistemes Gestors de Bases de Dades**
1. Permeten als usuaris emmagatzemar dades, accedir a elles i actualitzar-los de forma senzilla i amb un gran rendiment. Ocultant així la complexitat i les característiques físiques dels dispositius d'emmagatzemament. 
2. Garanteixen la integritat de les dades, respectant les regles i restriccions que dicta el programador de la BD. És a dir, no permeten accions que deixen dades incorrectes o incompletes. 
3. Integren junt amb el sistema operatiu, un sistema de seguretat que garanteix l'accés a la informació només a aquells usuaris que tinguin autorització. 
4. Proporciona un diccionari de metadades, el qual conté l'esquema de la base de dades, és a dir, com estan estructurades les dades en taules, registres i camps, les relacions entre les dades, usuaris, permisos,... . Aquest diccionari ha de ser accessible de la mateixa forma senzilla amb que s'accedeix a la resta de dades 
5. Permeten l'ús de transaccions, garantint que totes les operacions fetes per aquestes es realitzin correctament, i en cas d'alguna incidència, desfan els canvis sense cap problema addicional. 
6. Ofereixen estadístiques sobre l'ús del gestor, registrant operacions efectuades, consultes sol·licitades, operacions fallides i qualsevol tipus d'incidència. D'aquesta manera és possible monitoritzar l'ús de la base de dades cosa que permet analitzar possibles mals funcionaments. 
7. Permet concurrència, és a dir, diversos usuaris poden treballar en un mateix conjunt de dades. A més, té eines que controlen operacions conflictives d'accés o modificació d'una dada al mateix temps per part de diversos usuaris. 
8. Independitzen les dades de l'aplicació o usuari que les està utilitzant, fent més fàcil la seva migració cap altres plataformes. 
9. Ofereixen connectivitat amb l'exterior. D'aquesta manera es poden replicar i distribuir les bases de dades. A més, tots els SGBDs incorporen eines estàndard de connectivitat. El protocol ODBC (Open Database Connectivity) està bastant estès com a forma de comunicació entre BDs i aplicacions externes. 
10. Incorporen eines per a salvaguardar i restaurar la informació en cas de desastre. Alguns gestors, tenen sofisticats mecanismes per a poder establir l'estat d'una BD en qualsevol punt anterior en el temps. A més, també han d'oferir eines senzilles per a la importació i exportació automàtica de la informació.

**El llenguatge SQL**
	SQL es divideix en 4 sub-llenguatges, el conjunt d'ells permet al SGBD complir amb les funcions que marquen les lleis de Codd:

• Llenguatge DML (Data Manipulation Language): Llenguatge de manipulació de dades.
Aquest llenguatge permet amb 4 sentències senzilles seleccionar determinades dades
(SELECT), inserir dades (INSERT), modificar-les (UPDATE) o inclús esborrar-les
(DELETE). Ho veurem més endavant.

• Llenguatge DDL (Data Definition Language): Llenguatge de definició de dades. Aquest
llenguatge permet crear tota l'estructura d'una BD (des de taules fins a usuaris). Les
seves sentències són del tipus DROP (eliminar objectes) i CREATE (crear objectes). Ho
veurem més endavant.

• Llenguatge DCL (Data Control Language): Llenguatge de control de dades. Les seves
sentències (GRANT i REVOKE) permeten a l'administrador gestionar l'accés a les dades
que tenim a la BD.

• Llenguatge TCL (Transactions Control Language): Llenguatge de control de transaccions.
El propòsit d'aquest llenguatge es permetre executar diverses comandes de forma
simultània com si fos una comanda atòmica o indivisible. Si es possible executar totes
les comandes s'aplica la transacció (COMMIT), i si en algun pas de l'execució succeeix
alguna cosa inesperada, es poden desfer totes les accions realitzades (ROLLBACK).