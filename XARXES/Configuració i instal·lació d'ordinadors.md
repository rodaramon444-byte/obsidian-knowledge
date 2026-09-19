![[Pasted image 20260919122953.png]]
# Comprovació inicial del maquinari

Abans d’instal·lar res, és recomanable comprovar quin maquinari té l’equip.

Cal identificar principalment:

- CPU
- Memòria RAM
- SSD/HDD
- Targeta de xarxa Ethernet
- Adaptador Wi-Fi
- GPU
- Ports USB
- Sortides de vídeo
- Perifèrics
- Estat físic de l’equip

En un ordinador ja muntat també és important comprovar que la RAM i les unitats d’emmagatzematge siguin detectades correctament.

### Windows

Per a informació general:

```
msinfo32
```

Informació de DirectX, GPU, àudio, etc.:

```
dxdiag
```

Informació del sistema:

```
systeminfo
```

Des de PowerShell:

```
Get-ComputerInfo
```

Discos detectats:

```
Get-Disk
```

Adaptadors de xarxa:

```
Get-NetAdapter
```

---

# 3. BIOS / UEFI

Tecles:

`F2` · `F10` · `F12` · `DEL` · `ESC`

Depèn del fabricant.

## Què hauríem de comprovar?

### Ordre d’arrencada

Si instal·larem Windows des d’un USB, hem de poder arrencar des d’aquest dispositiu.

Exemple:

```
Boot Priority

1. USB
2. NVMe SSD
3. SATA SSD
4. Network/PXE
```

Després de la instal·lació podem tornar a deixar el SSD com a primera opció.

### UEFI

En equips moderns és preferible utilitzar:

```
Boot Mode: UEFI
```

en lloc del mode Legacy/CSM.

### Secure Boot

En una instal·lació estàndard de Windows 11 normalment:

```
Secure Boot: Enabled
```

### TPM

Windows 11 utilitza TPM 2.0 dins dels seus requisits habituals.

Pot aparèixer a la BIOS amb noms diferents segons el fabricant, per exemple:

```
TPM
Intel PTT
AMD fTPM
```

---

# 4. Instal·lació de Windows

## 4.1. Crear USB d’instal·lació

Podem preparar un USB d’instal·lació de Windows mitjançant eines oficials de Microsoft o utilitats de creació d’USB arrancables.

Normalment necessitarem:

```
USB
   ↓
ISO / instal·lador de Windows
   ↓
Arrencar ordinador des de l'USB
   ↓
Instal·lar Windows
```

## 4.2. Particions

Durant una instal·lació neta ens podem trobar diverses particions existents.

En un equip que **es pugui esborrar completament**, podem eliminar les particions de la instal·lació anterior i deixar:

```
Espai sense assignar
```

Windows crearà automàticament les particions necessàries.

**Important:** abans d’eliminar particions d’un ordinador d’un client, cal assegurar-se que existeixi una còpia de seguretat i que estigui autoritzat esborrar les dades.

---

# 5. Configuració inicial de Windows

Una vegada instal·lat Windows, hem de comprovar diversos elements.

## 5.1. Nom de l’ordinador

En una empresa és habitual utilitzar noms identificatius.

Per exemple:

```
PC-RECEPCIO-01
PC-ADMIN-02
PC-TALLER-01
PORTATIL-GERENCIA
```

Consultar el nom actual:

```
hostname
```

També:

```
echo %COMPUTERNAME%
```

Amb PowerShell:

```
$env:COMPUTERNAME
```

Canviar-lo amb PowerShell:

```
Rename-Computer -NewName "PC-ADMIN-01" -Restart
```

---

# 6. Drivers

Després d’instal·lar Windows hem de comprovar que tots els dispositius tinguin els drivers correctes.

Podem obrir l’Administrador de dispositius amb:

```
devmgmt.msc
```

Cal revisar especialment:

```
Adaptador Ethernet
Wi-Fi
Bluetooth
GPU
Àudio
Chipset
USB
```

Un símbol d’advertència ⚠️ acostuma a indicar un problema amb el dispositiu o el seu controlador.

Sempre que sigui possible, els drivers s’han d’obtenir mitjançant **Windows Update o el fabricant oficial del dispositiu/equip**.

---

# 7. Configuració de xarxa

Aquesta és probablement una de les parts que més et servirà durant les pràctiques.

## 7.1. Consultar configuració IP

```
ipconfig
```

Informació completa:

```
ipconfig /all
```

Exemple:

```
IPv4:       192.168.1.25
Màscara:    255.255.255.0
Gateway:    192.168.1.1
DNS:        192.168.1.1
```

Aquí tenim quatre conceptes fonamentals:

**IP:** identifica l’ordinador dins de la xarxa.

**Màscara:** determina quina part de l’adreça identifica la xarxa.

**Gateway:** dispositiu al qual enviem el trànsit destinat a altres xarxes; normalment serà el router.

**DNS:** permet convertir noms com `google.com` en adreces IP.

---

## 7.2. DHCP

Normalment els ordinadors d’una xarxa obtenen automàticament:

```
IP
Màscara
Gateway
DNS
```

mitjançant **DHCP**.

Esquema simplificat:

```
PC
 │
 │ Sol·licita configuració
 ▼
Servidor DHCP / Router
 │
 │ Assigna IP
 ▼
192.168.1.25
```

Podem renovar l’adreça DHCP amb:

```
ipconfig /release
```

i després:

```
ipconfig /renew
```

---

# 8. Comandes essencials de diagnòstic

Aquest apartat és especialment important.

## PING

Comprova si tenim comunicació amb un altre dispositiu.

```
ping 192.168.1.1
```

Per provar Internet:

```
ping 8.8.8.8
```

Per provar també la resolució DNS:

```
ping google.com
```

Això ens permet fer un diagnòstic molt ràpid.

### Exemple

Si:

```
ping 8.8.8.8
```

funciona, però:

```
ping google.com
```

no funciona, una de les primeres coses que hauríem de comprovar és **el DNS**.

---

## IPCONFIG

Configuració IP:

```
ipconfig
```

Configuració completa:

```
ipconfig /all
```

Alliberar IP DHCP:

```
ipconfig /release
```

Demanar una IP nova:

```
ipconfig /renew
```

Mostrar memòria cau DNS:

```
ipconfig /displaydns
```

Netejar-la:

```
ipconfig /flushdns
```

---

## NSLOOKUP

Permet diagnosticar DNS.

```
nslookup google.com
```

També podem consultar un servidor DNS concret:

```
nslookup google.com 8.8.8.8
```

---

## TRACERT

Mostra el recorregut dels paquets fins a una destinació.

```
tracert google.com
```

És útil per detectar en quin punt deixa de respondre una connexió.

---

## ARP

Mostra la taula ARP:

```
arp -a
```

Ens permet veure associacions del tipus:

```
IP                  MAC
192.168.1.1    →    AA-BB-CC-DD-EE-FF
192.168.1.20   →    11-22-33-44-55-66
```

---

## NETSTAT

Connexions de xarxa:

```
netstat
```

Connexions i ports:

```
netstat -ano
```

Exemple:

```
TCP    192.168.1.25:50432    142.250.x.x:443    ESTABLISHED
```

El PID que apareix al final ens permet identificar quin procés està utilitzant aquella connexió.

```
tasklist
```

O concretament:

```
tasklist | findstr 1234
```

---

# 9. Procediment quan un PC no té Internet

Aquesta és una de les parts que jo tindria **marcada especialment al document**, perquè et pot salvar bastant durant les pràctiques.

Segueix sempre una seqüència lògica:

```
1. Comprovar cable / Wi-Fi
          ↓
2. ipconfig /all
          ↓
3. Comprovar si tenim una IP correcta
          ↓
4. ping 127.0.0.1
          ↓
5. ping a la nostra IP
          ↓
6. ping al gateway
          ↓
7. ping 8.8.8.8
          ↓
8. ping google.com
          ↓
9. nslookup google.com
          ↓
10. tracert si continua el problema
```