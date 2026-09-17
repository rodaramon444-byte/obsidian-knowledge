### **PART 1: Recerca, Selecció i Justificació**

**URL:** [Wallapop](https://es.wallapop.com/?utm_source=chatgpt.com)

Hem triat **Wallapop**. Compleix els requisits perquè permet registrar-se i iniciar sessió amb un compte d’usuari. També permet crear i modificar dades publicant productes per vendre, amb fotos, descripció i preu. Finalment, mostra un catàleg amb els productes publicats pels diferents usuaris, que es poden buscar i filtrar.

### **PART 2: Anàlisi Tècnica**

## 1. Anàlisi general

### Per què és una aplicació web?

Wallapop és una aplicació web perquè podem accedir-hi directament des d’un navegador i utilitzar les seves funcions sense necessitat d’instal·lar cap programa. La pàgina permet iniciar sessió, buscar productes, publicar anuncis i comunicar-se amb altres usuaris.

### Avantatges i desavantatge

Un primer avantatge és que **no cal instal·lar res**, ja que podem entrar a Wallapop des del navegador. Un altre avantatge és que **podem accedir al nostre compte des de diferents dispositius** simplement iniciant sessió.

Com a desavantatge, **necessitem connexió a Internet** per poder consultar els productes, publicar anuncis o comunicar-nos amb altres usuaris.

## 2. Arquitectura Client-Servidor

### Front-End

A la banda del client podem trobar elements com **el cercador de productes**, que permet escriure què volem buscar; **els botons i menús de navegació**, que permeten moure’ns per la web; i **les fitxes dels productes**, on veiem les fotografies, el preu i la informació de cada anunci.

### Back-End

A la banda del servidor es realitzen processos que nosaltres no veiem directament. Per exemple, **comprovar les dades quan un usuari inicia sessió**, **buscar a la base de dades els productes que coincideixen amb una cerca** i **guardar un anunci a la base de dades quan un usuari publica un producte**.

### Tecnologies Client

Mitjançant les eines de desenvolupador del navegador podem observar que la pàgina utilitza tecnologies pròpies del desenvolupament web com **HTML, CSS i JavaScript**.

**[INSERIR AQUÍ LA CAPTURA DE PANTALLA DE LES EINES DE DESENVOLUPADOR]**

## 3. Anàlisi per Capes i MVC

### Diagrama de capes

**PRESENTACIÓ**  
↓  
Pàgina web que veu i utilitza l’usuari: cercador, productes, botons, imatges, etc.

**LÒGICA**  
↓  
Processa les accions de l’usuari, com iniciar sessió, fer una cerca o publicar un producte.

**DADES**  
↓  
Base de dades on es guarda la informació dels usuaris, productes, anuncis, missatges, etc.

La capa de **presentació** és la part amb la qual interactua l’usuari. La capa de **lògica** s’encarrega de processar les peticions i decidir què s’ha de fer. Finalment, la capa de **dades** permet guardar i recuperar la informació necessària.

### Hipòtesi del patró MVC

Com a exemple utilitzarem l’acció de **buscar un producte a Wallapop**.

**Controlador:** quan l’usuari escriu un producte i prem el botó de cercar, el controlador rep aquesta petició i demana la informació necessària.

**Model:** s’encarrega de consultar les dades i buscar els productes que coincideixen amb el que ha escrit l’usuari.

**Vista:** rep els resultats i els mostra a la pantalla en forma de llista de productes amb les seves fotografies, noms, preus i altra informació.