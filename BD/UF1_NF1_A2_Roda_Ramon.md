
# Introducció

Amb el programa BASE del LibreOffice crea una base de dades anomenada
**"ROHIRRIM"** la qual contindrà tots els cavalls de l'exercit de rohan
i els seus cavallers coneguts com a Rohirrim**.** Dins d'aquesta base de
dades realitza les accions que benen detallades al següent apartat.

# 

# Tasques a realitzar

1\. Crea una taula anomenada CAVALLS amb els camps nom, tipus, raça, pes
i color. Tingues en compte d'afegir a la taula un camp clau. I fixaràs
que al camp tipus només puguis posar dos valors, 'cavall' i 'poni'.

Per a fixar que al camp tipus i que només pugui posar els dos valors, he
creat una taula ‘tipus’ on hi ha els dos tipus, i després relaciono codi
de la taula tipus amb tipus de la taula cavalls i creare un quadre de
llista perquè llisti les dues opcions o poni o cavall:

Tula cavalls:

<img src="attachments/UF1_NF1_A2/media/image56.png"
style="width:2.9375in;height:1.48958in" />

Taula tipus:

<img src="attachments/UF1_NF1_A2/media/image9.png"
style="width:2.85417in;height:0.80208in" />

i inserixo els tipus:

<img src="attachments/UF1_NF1_A2/media/image51.png"
style="width:2.26042in;height:0.94792in" />

2\. Insereix 5 registres a la taula CAVALLS.

Per inserir registres, creo un formulari amb la taula cavalls:

<img src="attachments/UF1_NF1_A2/media/image7.png"
style="width:6.10313in;height:3.14173in" />

A continuació per poder elegir el tipus de cavall, desagrupem el camp
tipus i esborrem on sortiria el text:

<img src="attachments/UF1_NF1_A2/media/image4.png"
style="width:4.04065in;height:4.28266in" />

Esborrem aquest:

<img src="attachments/UF1_NF1_A2/media/image40.png"
style="width:3.34375in;height:2.55208in" />

I inserim un quadre combinat tal com es mostra a la imatge:

<img src="attachments/UF1_NF1_A2/media/image41.png"
style="width:4.19792in;height:3.46875in" />

Seleccionem la taula que volem, amb el meu cas la taula tipus:

<img src="attachments/UF1_NF1_A2/media/image44.png"
style="width:4.67466in;height:3.99625in" />

I seleccionem el camp que volem que es mostrin les dades:

<img src="attachments/UF1_NF1_A2/media/image57.png"
style="width:4.80364in;height:3.98473in" />

Per poder veure quina dada hem seleccionat, premem que si, i on volem
que es desin les dades seleccionades, amb el meu cas ‘tipus’:

<img src="attachments/UF1_NF1_A2/media/image55.png"
style="width:4.91823in;height:1.76295in" />

3\. Crea una consulta per a veure els animals tipus 'cavall'.

Creem la consulta amb vista disseny i seleccionem les taules cavalls i
tipus per inserir els camps de la taula cavall i el camp tipus:

Al camp tipus, filtrem per cavall amb cometes tal com es mostra:

<img src="attachments/UF1_NF1_A2/media/image10.png"
style="width:5.60573in;height:5.54016in" />

Resultat amb vista normal:

<img src="attachments/UF1_NF1_A2/media/image14.png"
style="width:4.8125in;height:1.36458in" />

4\. Crea una taula anomenada CAVALLERS amb els camps nom, casa,
data_naixement, regió, maestria. Tingues en compte d'afegir a la taula
un camp clau. I fixaràs que al camp maestria només puguis posar tres
valors, 'espasa', 'arc' i 'llança'.

Taula Cavallers:

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

<img src="attachments/UF1_NF1_A2/media/image45.png"
style="width:3.04167in;height:0.21875in" />

<img src="attachments/UF1_NF1_A2/media/image6.png"
style="width:2.51042in;height:0.51042in" />

Camp nom, amb tipus text, longitud 20 i entrada requerida sí:

<img src="attachments/UF1_NF1_A2/media/image3.png"
style="width:2.69792in;height:0.25in" />

<img src="attachments/UF1_NF1_A2/media/image17.png"
style="width:2.32292in;height:0.77083in" />

Camp casa, amb tipus text, longitud 30 i entrada requerida sí:

<img src="attachments/UF1_NF1_A2/media/image18.png"
style="width:2.72917in;height:0.1875in" />

<img src="attachments/UF1_NF1_A2/media/image22.png"
style="width:2.42708in;height:0.86458in" />

Camp data, amb tipus data i entrada requerida sí:

<img src="attachments/UF1_NF1_A2/media/image50.png"
style="width:3.5in;height:0.21875in" />

<img src="attachments/UF1_NF1_A2/media/image42.png"
style="width:2.26042in;height:0.54167in" />

Camp regió amb tipus text, longitud 20 i entrada requerida sí:

<img src="attachments/UF1_NF1_A2/media/image20.png"
style="width:3.25in;height:0.20833in" />

<img src="attachments/UF1_NF1_A2/media/image32.png"
style="width:2.55208in;height:0.85417in" />

Camp codi maestria amb tipus enter (clau forana)

<img src="attachments/UF1_NF1_A2/media/image11.png"
style="width:3.32292in;height:0.20833in" />

Talua Tipus maestria:

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

<img src="attachments/UF1_NF1_A2/media/image25.png"
style="width:2.91667in;height:0.19792in" />

<img src="attachments/UF1_NF1_A2/media/image33.png"
style="width:2.875in;height:0.875in" />

Camp tipus maestria, amb tipus text i longitud 20:

<img src="attachments/UF1_NF1_A2/media/image38.png"
style="width:2.9375in;height:0.21875in" />

<img src="attachments/UF1_NF1_A2/media/image8.png"
style="width:2.57448in;height:0.88542in" />

I hem d’inserir els tipus de maestries amb vista normal:

<img src="attachments/UF1_NF1_A2/media/image12.png"
style="width:2.61458in;height:1.3125in" />

5\. Insereix 5 registres a la taula CAVALLERS.

Per inserir registres, creem un formulari de la taula cavallers, inserim
totes les dades:

<img src="attachments/UF1_NF1_A2/media/image21.png"
style="width:2.44792in;height:0.78125in" />

<img src="attachments/UF1_NF1_A2/media/image2.png"
style="width:2.30208in;height:1.80208in" />

Tot seguit afegim un quadre de llista com hem fet anteriorment:

<img src="attachments/UF1_NF1_A2/media/image31.png"
style="width:4.1875in;height:2.29167in" />

I seleccionem la taula maestria:

<img src="attachments/UF1_NF1_A2/media/image26.png"
style="width:2.51042in;height:0.76042in" />

amb el camp tipus maestria:

<img src="attachments/UF1_NF1_A2/media/image13.png"
style="width:5.08333in;height:1.10417in" />

I inserim els registres:

<img src="attachments/UF1_NF1_A2/media/image15.png"
style="width:3.23958in;height:2.01042in" />

6\. Crea una consulta per mostrar quins són els cavallers que tenen
maestria amb arc i els anys d'experiència. La realització d'aquesta
consulta provocarà la modificació de la taula CAVALLERS.

Inserim les taules T_cavallers i T_Maestria:

<img src="attachments/UF1_NF1_A2/media/image23.png"
style="width:4.92708in;height:2.96875in" />

I inserim els camps necessaris de la taula cavallers, i el camp tipus
maestria:

<img src="attachments/UF1_NF1_A2/media/image37.png"
style="width:6.69272in;height:1.79167in" />

On a criteri i de maestria filtrem per Arc:

<img src="attachments/UF1_NF1_A2/media/image28.png"
style="width:1.39583in;height:2.0625in" />

Resultat:

<img src="attachments/UF1_NF1_A2/media/image54.png"
style="width:6.27083in;height:0.90625in" />

7\. Crea una taula anomenada EOREDS amb els camps DataAlta, CodiCavaller
i NomMariscal.

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

<img src="attachments/UF1_NF1_A2/media/image27.png"
style="width:2.97917in;height:0.27083in" />

<img src="attachments/UF1_NF1_A2/media/image52.png"
style="width:2.5in;height:0.89583in" />

Camp DataAlta amb tipus Data, entrada requerida si:

<img src="attachments/UF1_NF1_A2/media/image49.png"
style="width:2.95833in;height:0.27083in" />

<img src="attachments/UF1_NF1_A2/media/image53.png"
style="width:2.95833in;height:1.4237in" />

Camp CodiCavaller amb tipus Enter:

<img src="attachments/UF1_NF1_A2/media/image1.png"
style="width:3.14583in;height:0.22917in" />

<img src="attachments/UF1_NF1_A2/media/image58.png"
style="width:3.42708in;height:1.4375in" />

Camp NomMariscal amb tipus text, entrada requerida si i longitud 20:

<img src="attachments/UF1_NF1_A2/media/image46.png"
style="width:2.72917in;height:0.19792in" />

<img src="attachments/UF1_NF1_A2/media/image24.png"
style="width:3.29167in;height:0.92708in" />

8\. Crea les relacions entre la taula EOREDS, CAVALLERS i CAVALLS.

El meu raonament és el següent, com els cavallers tenen un cavall,
relacionem 1 a 1, de tal manera que cada cavall estarà assignat a un
cavaller, i he relacionat el codi de la taula eored a codieored de la
taula cavaller de tal manera que així es puga assignar i saber de quin
eored és cada cavaller:

<img src="attachments/UF1_NF1_A2/media/image47.png"
style="width:6.69272in;height:3.88889in" />

9\. Insereix 3 Eoreds diferents, i per a cadascun d'ells tres Cavallers.

Creem formulari amb auxiliar i afegim els camps codi, nomeored,
nommariscal i dataalta de la taula eoreds:

<img src="attachments/UF1_NF1_A2/media/image36.png"
style="width:4.19792in;height:2.0625in" />

Afegim subformulari amb l'opció següent:

<img src="attachments/UF1_NF1_A2/media/image39.png"
style="width:5.1875in;height:2.60417in" />

I afegim els camps codi, nom, casa, data, regio, maestria i anys de la
taula cavallers:

<img src="attachments/UF1_NF1_A2/media/image5.png"
style="width:4.40625in;height:3.01042in" />

Un cop això fet, crearem un quadre de llista per a la maestria del
cavaller tal com havíem fet anteriorment:

<img src="attachments/UF1_NF1_A2/media/image5.png"
style="width:3.22677in;height:2.20458in" />

I seleccionem la taula maestria:

<img src="attachments/UF1_NF1_A2/media/image26.png"
style="width:2.39739in;height:0.72618in" />

Amb el camp tipus maestria:

<img src="attachments/UF1_NF1_A2/media/image13.png"
style="width:4.63177in;height:1.00608in" />

Un cop fet, afegim els registres:

<img src="attachments/UF1_NF1_A2/media/image16.png"
style="width:4.25928in;height:3.93245in" />

10\. Crea un informe per llistar els cavallers agrupats baix del seu
Eored i Mariscal.

Per fer el formulari i que sortien els cavallers llistats amb el seu
respectiu eored i mariscal, creem una consulta de les taules cavallers i
eoreds amb els camps “nomcavaller, NomEored i nommariscal”:

<img src="attachments/UF1_NF1_A2/media/image48.png"
style="width:6.69272in;height:3.875in" />

Ara creem l'informe de la consulta amb auxiliar i seleccionem tots els
camps:

<img src="attachments/UF1_NF1_A2/media/image29.png"
style="width:4.04167in;height:2.0625in" />

A agrupació seleccionem els camps nomeored i nommariscal:

<img src="attachments/UF1_NF1_A2/media/image43.png"
style="width:6.61458in;height:2.19792in" />

Seguidament, a opcions d’agrupació, posem els camps de la següent
manera:

<img src="attachments/UF1_NF1_A2/media/image30.png"
style="width:6.69272in;height:3.84722in" />

Resultat final:

<img src="attachments/UF1_NF1_A2/media/image34.png"
style="width:4.15625in;height:4.90625in" />

**<u>Recordatori:</u>** Penseu que les captures han d'anar acompanyades
sempre d'un text
