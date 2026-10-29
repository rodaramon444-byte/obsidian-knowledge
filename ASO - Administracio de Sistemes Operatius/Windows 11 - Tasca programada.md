**ADMINISTRACIÓ DE  
SISTEMES OPERATIUS**

**Automatització de tasques administratives a Windows 11**

Tasca programada i script PowerShell

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Autor  
**Ramon Roda Adame  
  
**Assignatura  
**Administració de sistemes operatius  
  
**Entorn de treball  
**Microsoft Windows 11 Pro

ÍNDEX

| 1\. Introducció | 3   |
|-----------------|-----|

| 1.1 Objectiu de la pràctica | 3   |
|-----------------------------|-----|

| 1.2 Flux de funcionament | 3   |
|--------------------------|-----|

| 2\. Preparació de l’entorn de treball | 4   |
|---------------------------------------|-----|

| 2.1 PowerShell amb privilegis d’administrador | 4   |
|-----------------------------------------------|-----|

| 2.2 Creació del directori C:\Ramon | 4   |
|------------------------------------|-----|

| 3\. Creació de l’script PowerShell | 5   |
|------------------------------------|-----|

| 3.1 Creació de ramon.ps1 | 5   |
|--------------------------|-----|

| 3.2 Informació recopilada | 6   |
|---------------------------|-----|

| 4\. Verificació manual de l’script | 6   |
|------------------------------------|-----|

| 4.1 Execució manual | 6   |
|---------------------|-----|

| 4.2 Comprovació del fitxer generat | 7   |
|------------------------------------|-----|

| 5\. Resultat de l’informe del sistema | 7   |
|---------------------------------------|-----|

| 6\. Automatització amb el Programador de tasques | 8   |
|--------------------------------------------------|-----|

| 7\. Comprovacions finals | 8   |
|--------------------------|-----|

# 1. Introducció

## 1.1 Objectiu de la pràctica

L’objectiu d’aquesta pràctica és preparar a Windows 11 un procés
administratiu automatitzat. El sistema utilitzarà un script de
PowerShell ubicat a C:\Ramon\ramon.ps1 per recopilar informació tècnica
de l’equip i generar l’informe C:\Ramon\informe-sistema.txt.
Posteriorment, l’execució de l’script s’automatitzarà mitjançant el
Programador de tasques de Windows amb privilegis d’administrador.

En aquesta fase s’ha preparat el directori de treball, s’ha creat
l’script i se n’ha verificat manualment el funcionament abans de
configurar l’execució automàtica.

## 1.2 Flux de funcionament

**Inici de Windows → Tasca programada → C:\Ramon\ramon.ps1  
→ Recollida d’informació → C:\Ramon\informe-sistema.txt**

# 2. Preparació de l’entorn de treball

## 2.1 Obertura de PowerShell amb privilegis d’administrador

S’ha iniciat Windows PowerShell amb permisos d’administrador. Aquesta
elevació de privilegis permet treballar amb la configuració del sistema
i prepara l’entorn per a l’execució posterior de la tasca programada amb
els privilegis més alts.

## 2.2 Creació i verificació del directori C:\Ramon

Primer s’ha comprovat si el directori de treball ja existia:

Test-Path "C:\Ramon"

El resultat inicial ha estat False, de manera que el directori encara no
existia. S’ha creat amb la comanda següent:

New-Item -Path "C:\Ramon" -ItemType Directory -Force

Finalment, s’ha repetit la comprovació:

Test-Path "C:\Ramon"

El segon resultat ha estat True, fet que confirma que el directori
C:\Ramon s’ha creat correctament.

<img
src="/home/alumnat/Documents/Obsidian Vault/attachments/aso-tasca-programada/media/image1.png"
style="width:6.25in;height:3.44288in" />

*Figura 1. Creació i verificació del directori C:\Ramon des de
PowerShell executat com a administrador.*

A la captura s’observa la seqüència completa: Test-Path retorna False
abans de la creació, New-Item genera el directori i la comprovació
posterior retorna True. Per tant, queda validat el directori que
allotjarà l’script i el fitxer d’informe.

# 3. Creació de l’script PowerShell

## 3.1 Creació del fitxer ramon.ps1

El fitxer de treball s’ha definit a la ruta C:\Ramon\ramon.ps1. En obrir
aquesta ruta per primera vegada amb el Bloc de notes, Windows indica que
el fitxer no existeix i ofereix crear-ne un de nou.

<img
src="/home/alumnat/Documents/Obsidian Vault/attachments/aso-tasca-programada/media/image2.png"
style="width:6.25in;height:4.67557in" />

*Figura 2. Missatge del Bloc de notes en crear per primera vegada el
fitxer C:\Ramon\ramon.ps1.*

Aquesta comprovació permet confirmar tant el nom com la ubicació de
l’script abans d’introduir-ne el contingut.

Un cop creat el fitxer, s’hi ha introduït el codi PowerShell encarregat
de generar l’informe del sistema.

<img
src="/home/alumnat/Documents/Obsidian Vault/attachments/aso-tasca-programada/media/image3.png"
style="width:6.25in;height:4.95547in" />

*Figura 3. Edició del contingut de l’script ramon.ps1 amb el Bloc de
notes.*

A la captura es pot veure la variable \$Informe apuntant a
C:\Ramon\informe-sistema.txt i les primeres instruccions que escriuen la
capçalera de l’informe i consulten informació del sistema mitjançant
PowerShell.

## 3.2 Informació recopilada per l’script

L’script està preparat per registrar la data i l’hora d’execució,
l’usuari actual, el nom de l’ordinador, el sistema operatiu i la seva
arquitectura, el processador, la memòria RAM, les adreces IPv4, l’espai
disponible als discos i els serveis en execució.

Per obtenir aquestes dades s’utilitzen cmdlets de PowerShell com
Get-CimInstance, Get-NetIPAddress i Get-Service. La informació es desa
al fitxer de sortida mitjançant Out-File.

# 4. Verificació manual de l’script

## 4.1 Execució manual

Abans de crear la tasca programada s’ha comprovat que l’script funciona
de manera independent. Des d’un PowerShell amb privilegis
d’administrador s’ha executat:

powershell.exe -ExecutionPolicy Bypass -File "C:\Ramon\ramon.ps1"

L’opció -ExecutionPolicy Bypass s’aplica únicament a aquesta execució i
permet provar l’script sense modificar de manera permanent la política
d’execució configurada al sistema.

## 4.2 Comprovació del fitxer generat

Després de l’execució s’ha verificat l’existència del fitxer de sortida:

Test-Path "C:\Ramon\informe-sistema.txt"

PowerShell ha retornat True, confirmant que l’informe s’ha generat
correctament.

<img
src="/home/alumnat/Documents/Obsidian Vault/attachments/aso-tasca-programada/media/image4.png"
style="width:6.25in;height:4.33277in" />

*Figura 4. Execució manual de ramon.ps1 i verificació de la creació de
l’informe.*

La captura mostra l’execució de ramon.ps1 sense errors i, tot seguit, la
comprovació de C:\Ramon\informe-sistema.txt amb resultat True. Aquesta
prova valida el funcionament manual de l’script abans d’automatitzar-lo.

# 5. Resultat de l’informe del sistema

Finalment, s’ha obert C:\Ramon\informe-sistema.txt amb el Bloc de notes
per revisar les dades generades.

<img
src="/home/alumnat/Documents/Obsidian Vault/attachments/aso-tasca-programada/media/image5.png"
style="width:6.25in;height:4.96858in" />

*Figura 5. Informe del sistema generat automàticament per l’script
ramon.ps1.*

A la captura s’observen dades reals obtingudes durant la prova: l’usuari
roda, l’equip DESKTOP-6N25E61, Microsoft Windows 11 Pro, la versió
10.0.26200, arquitectura de 64 bits i un processador Intel(R) Core(TM)
i7-10700 CPU @ 2.90GHz. També s’hi inicia l’apartat corresponent a la
memòria RAM.

**RESULTAT DE LA FASE:** el directori de treball existeix, l’script està
creat, l’execució manual funciona i l’informe es genera correctament amb
informació del sistema.

# 6. Automatització mitjançant el Programador de tasques

Pendent de realitzar. En aquesta fase es crearà una tasca programada de
Windows que executi C:\Ramon\ramon.ps1 automàticament amb privilegis
d’administrador. La configuració es documentarà amb les captures
corresponents a mesura que s’executi la pràctica.

# 7. Comprovacions finals

Pendent de realitzar. Un cop configurada la tasca programada, es
comprovarà l’execució automàtica i es validarà que l’informe es torna a
generar correctament sense intervenció manual.
