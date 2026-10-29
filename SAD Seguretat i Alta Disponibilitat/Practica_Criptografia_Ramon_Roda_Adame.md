# Pràctica de criptografia

ASIX2 · M11 · UF1 · Pràctica 3.1

Autor: Ramon Roda Adame

Data: 29 d’octubre de 2026

OpenSSL · ZIP de prova · Funcions hash

Execució: Kali sobre VirtualBox; accés SSH des d’Ubuntu.

Enunciat: ASIX2_M11_UF1_Pt3.1_Encriptacio.odt. Els originals es conserven sense modificar.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Índex

03   Preparació i comprovació de l’entorn

04   Tasca 1 · Part 1 · Xifrat binari AES-256-CBC

05   Tasca 1 · Part 1 · Representació Base64

06   Tasca 1 · Part 2 · Desxifrat i verificació

07   Tasca 2 · Part 1 · Crear i xifrar fitxers

08   Tasca 2 · Part 1 · Prova amb clau incorrecta

09   Tasca 2 · Part 2 · Opcions de recuperació

10   Tasca 2 · Part 2 · Recuperació de file-1.zip

11   Tasca 2 · Part 2 · Recuperació de file-2.zip

12   Tasca 2 · Part 2 · Recuperació de file-3.zip

13   Tasca 2 · Part 2 · Recuperació de file-4.zip

14   Tasca 2 · Part 2 · Recuperació de file-5.zip

15   Tasca 2 · Interpretació dels temps

16   Tasca 3 · Part 1 · Hashos MD5 i SHA-1

17   Tasca 3 · Part 2 · Verificació SHA-256

18   Incidències i punts pendents

19   Informació consultada i fitxers lliurats

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Preparació i comprovació de l’entorn

S’ha llegit l’enunciat complet abans d’executar els exercicis. Connexió amb ssh -o BatchMode=yes -i ~/.ssh/kali_practica ramon@192.168.203.169.

Carpeta nova a Kali: /home/ramon/Practica_Criptografia_20261029_162550. No s’ha reutilitzat treball anterior. OpenSSL 3.6.2, ZIP 3.0 i fcrackzip 1.0 estan disponibles. S’han copiat sample.png i hash_sha256.txt sense alterar-los.

Les captures són pantalles reals de QTerminal a Kali, obtingudes amb VBoxManage controlvm Kali screenshotpng. Al document s’insereixen retalls de la zona del terminal per llegibilitat; les captures completes es guarden a captures/. Les ordres s’executen amb bash i set -x, no són text dibuixat en una imatge.

![Captura real de Kali: Entorn i eines. Registre: registres/01_entorn.log.](captures/retalls/01_entorn.png){width=9.8in}

Captura real de Kali: Entorn i eines. Registre: registres/01_entorn.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 1 · Part 1 · Xifrat binari AES-256-CBC

S’ha creat document.txt amb el text «Practica de criptografia. Ramon Roda Adame.» i un salt de línia final. L’ordre openssl aes-256-cbc -in document.txt -out message-bin.enc xifra el contingut.

cat mostra bytes binaris, no el missatge original; alguns bytes poden tenir efectes de control al terminal. xxd en permet veure una representació hexadecimal estable. La capçalera Salted__ no és el text en clar.

S’utilitza -pass file:.aes_pass per automatitzar la contrasenya de laboratori, amb permisos 600. És una adaptació de l’entrada interactiva; no es modifica l’algoritme. El resultat binari es conserva separat del resultat Base64.

![Captura real de Kali: Tasca 1.1 · AES binari. Registre: registres/02_aes_binari.log.](captures/retalls/02_aes_binari.png){width=9.8in}

Captura real de Kali: Tasca 1.1 · AES binari. Registre: registres/02_aes_binari.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 1 · Part 1 · Representació Base64

Correcció: openssl aes-256-cbc -a -in document.txt -out message.enc. A l’enunciat manca el guionet de -out.

Ara cat sí que mostra una cadena ASCII llegible, però no mostra el missatge original. AES xifra i després -a codifica els bytes xifrats en Base64. És incorrecte dir que -a aplica un «mètode de xifrat diferent a Base64»: Base64 és una codificació reversible sense clau.

Benefici: facilita copiar, enviar i inserir el resultat en canals de text. No aporta confidencialitat addicional i augmenta la mida aproximadament un terç.

![Captura real de Kali: Tasca 1.1 · AES amb Base64. Registre: registres/03_aes_base64.log.](captures/retalls/03_aes_base64.png){width=9.8in}

Captura real de Kali: Tasca 1.1 · AES amb Base64. Registre: registres/03_aes_base64.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 1 · Part 2 · Desxifrat i verificació

L’ordre openssl aes-256-cbc -a -d -in message.enc -out desxifrat.txt, amb la mateixa contrasenya, recupera el text. cat el mostra i cmp confirma que els dos fitxers són idèntics byte a byte. Per tant, s’ha desxifrat correctament.

L’opció -a és necessària perquè l’entrada està en Base64: primer es descodifica i després es desxifra amb AES.

OpenSSL avisa que la derivació de clau original és obsoleta. També s’ha comprovat una variant amb -pbkdf2 -iter 100000 i una sal aleatòria; el desxifrat coincideix. AES-CBC no autentica el missatge: aquesta demostració no constitueix per si sola un sistema complet de protecció.

![Captura real de Kali: Tasca 1.2 · Desxifrat i comparacio. Registre: registres/04_desxifrat.log.](captures/retalls/04_desxifrat.png){width=9.8in}

Captura real de Kali: Tasca 1.2 · Desxifrat i comparacio. Registre: registres/04_desxifrat.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 1 · Crear i xifrar fitxers

S’utilitza l’usuari real ramon i la MV Kali, en lloc d’analyst i CyberOps. Dins la carpeta nova s’ha creat Zip-Files, amb sample-1.txt, sample-2.txt i sample-3.txt.

Correcció de zip -e file-1.zip sample *: s’utilitza sample-*.txt, sense l’espai entre sample i *. Això selecciona només els tres textos i evita incloure ZIP creats abans.

S’han creat file-1.zip a file-5.zip amb B, BB, BBB, BBBB i BBBBB. Per automatitzar s’ha fet zip -P CLAU fitxer.zip sample-*.txt, equivalent a proporcionar la clau a zip -e. -P exposa la clau a la línia d’ordres: aquí només es fa servir amb dades i claus de laboratori. ZIP fa servir el xifrat tradicional ZipCrypto, que és feble.

![Captura real de Kali: Tasca 2.1 · Creacio dels ZIP propis. Registre: registres/05_zip.log.](captures/retalls/05_zip.png){width=9.8in}

Captura real de Kali: Tasca 2.1 · Creacio dels ZIP propis. Registre: registres/05_zip.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 1 · Prova amb clau incorrecta

unzip -t -P INCORRECTA file-1.zip rebutja els tres fitxers i retorna el codi 82. No s’han extret dades amb una clau incorrecta. La prova verifica el comportament real del ZIP xifrat.

Intercanvi amb un company: PENDENT. No s’han proporcionat els seus cinc ZIP. La recuperació següent s’ha fet exclusivament sobre els cinc ZIP creats en aquesta pràctica; no acredita cap intercanvi.

![Captura real de Kali: Tasca 2.1 · Contrasenya incorrecta. Registre: registres/06_zip_error.log.](captures/retalls/06_zip_error.png){width=9.8in}

Captura real de Kali: Tasca 2.1 · Contrasenya incorrecta. Registre: registres/06_zip_error.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Opcions de recuperació

S’ha consultat fcrackzip -h. -v mostra detall; -u comprova els candidats amb unzip; -l fixa la longitud; -b força la cerca exhaustiva; -c A restringeix l’alfabet a A–Z (26 caràcters).

L’ordre de l’enunciat fcrackzip -vul 1 file-1.zip és una agrupació vàlida de -v -u -l 1. S’han escrit les opcions separades i s’ha fixat explícitament l’alfabet per poder interpretar l’experiment. No s’ha proporcionat a fcrackzip la contrasenya ni un punt inicial proper a ella.

Mesura: time intern de Bash amb TIMEFORMAT i timeout 90s. real és temps de paret del procés local a Kali, incloent timeout i fcrackzip; no inclou SSH ni les captures. Una sola execució per longitud, resolució mostrada de mil·lisegons.

![Captura real de Kali: Tasca 2.2 · Opcions de fcrackzip. Registre: registres/07_ajuda.log.](captures/retalls/07_ajuda.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Opcions de fcrackzip. Registre: registres/07_ajuda.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Recuperació de file-1.zip

Ordre: time timeout 90s fcrackzip -b -c A -v -u -l 1 file-1.zip.

Contrasenya recuperada per fcrackzip: B. Temps real: 0.008 s; usuari: 0.004 s; sistema: 0.000 s. Codi de sortida: 0. No s’ha arribat al límit de 90 s.

Després de la recuperació, unzip -t amb la contrasenya trobada valida els tres fitxers amb resultat OK. Aquesta validació no substitueix la recuperació: a la captura es veu PASSWORD FOUND abans de comprovar el ZIP.

![Captura real de Kali: Tasca 2.2 · Recuperació real de 1 caràcters. Registre: registres/15_reintent_1.log.](captures/retalls/15_reintent_1.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Recuperació real de 1 caràcters. Registre: registres/15_reintent_1.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Recuperació de file-2.zip

Ordre: time timeout 90s fcrackzip -b -c A -v -u -l 2 file-2.zip.

Contrasenya recuperada per fcrackzip: BB. Temps real: 0.007 s; usuari: 0.004 s; sistema: 0.000 s. Codi de sortida: 0. No s’ha arribat al límit de 90 s.

Després de la recuperació, unzip -t amb la contrasenya trobada valida els tres fitxers amb resultat OK. Aquesta validació no substitueix la recuperació: a la captura es veu PASSWORD FOUND abans de comprovar el ZIP.

![Captura real de Kali: Tasca 2.2 · Recuperació real de 2 caràcters. Registre: registres/15_reintent_2.log.](captures/retalls/15_reintent_2.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Recuperació real de 2 caràcters. Registre: registres/15_reintent_2.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Recuperació de file-3.zip

Ordre: time timeout 90s fcrackzip -b -c A -v -u -l 3 file-3.zip.

Contrasenya recuperada per fcrackzip: BBB. Temps real: 0.008 s; usuari: 0.003 s; sistema: 0.002 s. Codi de sortida: 0. No s’ha arribat al límit de 90 s.

Després de la recuperació, unzip -t amb la contrasenya trobada valida els tres fitxers amb resultat OK. Aquesta validació no substitueix la recuperació: a la captura es veu PASSWORD FOUND abans de comprovar el ZIP.

![Captura real de Kali: Tasca 2.2 · Recuperació real de 3 caràcters. Registre: registres/15_reintent_3.log.](captures/retalls/15_reintent_3.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Recuperació real de 3 caràcters. Registre: registres/15_reintent_3.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Recuperació de file-4.zip

Ordre: time timeout 90s fcrackzip -b -c A -v -u -l 4 file-4.zip.

Contrasenya recuperada per fcrackzip: BBBB. Temps real: 0.010 s; usuari: 0.004 s; sistema: 0.001 s. Codi de sortida: 0. No s’ha arribat al límit de 90 s.

Després de la recuperació, unzip -t amb la contrasenya trobada valida els tres fitxers amb resultat OK. Aquesta validació no substitueix la recuperació: a la captura es veu PASSWORD FOUND abans de comprovar el ZIP.

![Captura real de Kali: Tasca 2.2 · Recuperació real de 4 caràcters. Registre: registres/15_reintent_4.log.](captures/retalls/15_reintent_4.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Recuperació real de 4 caràcters. Registre: registres/15_reintent_4.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Part 2 · Recuperació de file-5.zip

Ordre: time timeout 90s fcrackzip -b -c A -v -u -l 5 file-5.zip.

Contrasenya recuperada per fcrackzip: BBBBB. Temps real: 0.064 s; usuari: 0.036 s; sistema: 0.008 s. Codi de sortida: 0. No s’ha arribat al límit de 90 s.

Després de la recuperació, unzip -t amb la contrasenya trobada valida els tres fitxers amb resultat OK. Aquesta validació no substitueix la recuperació: a la captura es veu PASSWORD FOUND abans de comprovar el ZIP.

![Captura real de Kali: Tasca 2.2 · Recuperació real de 5 caràcters. Registre: registres/15_reintent_5.log.](captures/retalls/15_reintent_5.png){width=9.8in}

Captura real de Kali: Tasca 2.2 · Recuperació real de 5 caràcters. Registre: registres/15_reintent_5.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 2 · Interpretació dels temps

Fitxer | Longitud | Contrasenya recuperada | Real (s)

file-1.zip | 1 | B | 0.008

file-2.zip | 2 | BB | 0.007

file-3.zip | 3 | BBB | 0.008

file-4.zip | 4 | BBBB | 0.010

file-5.zip | 5 | BBBBB | 0.064

L’espai de cerca de longitud fixa és 26ⁿ: 26, 676, 17.576, 456.976 i 11.881.376 candidats. La posició de la clau també influeix: B repetida apareix relativament aviat en una cerca alfabètica. Aquests temps no corresponen a esgotar tot l’espai.

Els primers temps són molt petits i no creixen estrictament: arrencada, planificació de processos i verificació tenen pes. Amb cinc caràcters el temps observat augmenta fins a 0,064 s. Una mostra amb claus repetitives i només majúscules no permet predir el temps per a claus aleatòries llargues o alfabets amplis. Més longitud i més tipus de caràcters amplien l’espai de cerca.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 3 · Part 1 · Hashos MD5 i SHA-1

S’ha creat Hashos/document.txt amb «Hola Ramon» i un salt de línia. S’ha conservat document-original.txt i s’ha canviat només H per h.

MD5 original: b2e7d5ed92f002e9fb4312bd6d15f48d. MD5 modificat: e81e51ccae5c2950d41f76e39bfff6e4. Són molt diferents tot i canviar un sol caràcter: és l’efecte d’allau.

SHA-1 modificat: 6fae89e4db2febd0e67f0d286a6605b699327a3a. md5sum i sha1sum coincideixen amb OpenSSL quan s’apliquen als mateixos bytes. El format de presentació canvia, però l’algoritme i el resum són els mateixos.

Correcció: letter_to_grandma.txt no s’ha creat; s’utilitza document.txt. MD5 i SHA-1 estan obsolets per a usos que requereixen resistència a col·lisions. SHA-1 ja té col·lisions demostrades; la nota de l’enunciat és antiga. SHA-2 inclou SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224 i SHA-512/256 («share 512/224» és una errada). Un hash no és únic en sentit matemàtic: poden existir col·lisions.

![Captura real de Kali: Tasca 3.1 · MD5, canvi dun caracter i SHA-1. Registre: registres/13_hashos.log.](captures/retalls/13_hashos.png){width=9.8in}

Captura real de Kali: Tasca 3.1 · MD5, canvi dun caracter i SHA-1. Registre: registres/13_hashos.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Tasca 3 · Part 2 · Verificació SHA-256

S’han utilitzat els fitxers realment proporcionats: sample.png i hash_sha256.txt, en lloc de sample.img i sample.img_SHA256.sig.

Valor esperat i valor calculat, idèntics:

2a73c7a28ce96a0ff744236409c83d2597e9c43d1f63808c5e1e61a7965c76b8

Resultat real: COINCIDEIX. El fitxer coincideix amb la referència facilitada; no es detecta una alteració respecte d’aquesta referència. No es pot certificar només amb això l’origen ni reconstruir com es va fer la descàrrega.

Una referència hash manipulable juntament amb el fitxer no acredita autenticitat. Per validar l’editor cal una signatura digital verificada amb una clau pública de confiança. L’extensió .sig de l’exemple no converteix un simple resum en una signatura. El fitxer hash_sha256.txt no acaba amb salt de línia; per això el primer cat i la traça següent apareixen enganxats al registre.

![Captura real de Kali: Tasca 3.2 · Verificacio real de sample.png. Registre: registres/14_integritat.log.](captures/retalls/14_integritat.png){width=9.8in}

Captura real de Kali: Tasca 3.2 · Verificacio real de sample.png. Registre: registres/14_integritat.log.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Incidències i punts pendents

1. El sandbox local ha fallat abans d’executar ordres: bwrap: loopback: Failed RTM_NEWADDR: Operation not permitted. S’ha continuat amb les execucions fora del sandbox autoritzades per l’usuari.

2. /usr/bin/time no existeix a Kali. Els cinc intents inicials fallen amb codi 127, sense executar fcrackzip. Els registres i les captures d’aquests errors es conserven. S’ha resolt amb time intern de Bash i s’han repetit els cinc exercicis amb èxit (captures 15_reintent_*).

3. OpenSSL ha avisat de derivació de clau obsoleta: es documenta l’ordre original corregida i es verifica la variant PBKDF2.

4. No hi ha LibreOffice a l’amfitrió ni a Kali. El document editable es genera amb Pandoc i el PDF amb WeasyPrint a Kali, sense instal·lar eines.

5. Intercanvi amb company pendent per manca dels seus ZIP. No s’ha atacat cap arxiu aliè.

6. Les proves amb claus curtes i alfabet conegut són didàctiques; no són un benchmark general. Cap reintent ha esgotat els 90 s.

7. Les captures completes són originals; els retalls del document només eliminen espai buit o l’escriptori. Els scripts, registres, dades i ZIP propis s’adjunten per reproduir els passos.

```{=openxml}
<w:p><w:r><w:br w:type="page"/></w:r></w:p>
```

# Informació consultada i fitxers lliurats

Enunciat original: ASIX2_M11_UF1_Pt3.1_Encriptacio.odt, llegit completament.

OpenSSL, documentació oficial d’enc (Base64, sal i PBKDF2): https://github.com/openssl/openssl/blob/master/doc/man1/openssl-enc.pod.in

OpenSSL Wiki, Enc: https://wiki.openssl.org/index.php/Enc

NIST, Research Results on SHA-1 Collisions (2017): https://csrc.nist.gov/News/2017/Research-Results-on-SHA-1-Collisions

NIST, Policy on Hash Functions: https://csrc.nist.gov/Projects/Hash-Functions/NIST-Policy-on-Hash-Functions

Ajuda local executada: fcrackzip -h; versions d’OpenSSL i ZIP. Consulta web: 29/10/2026.

Lliurament: Practica_Criptografia_Ramon_Roda_Adame.docx (editable), PDF, font HTML i Markdown, captures completes i retalls identificats, registres .sh i .log, resultats.json i còpies dels fitxers generats a Kali. Les claus incloses són exclusivament de laboratori.

Els originals de la carpeta inicial no s’han modificat. manifest_originals.sha256 en registra els resums al final de l’execució.