
1.  Tasques a realitzar

2. Crea una taula anomenada CAVALLS amb els camps nom, tipus, raça, pes i color. Tingues en compte d'afegir a la taula un camp clau. I fixaràs que al camp tipus només puguis posar dos valors, 'cavall' i 'poni'.  

Per a fixar que al camp tipus i que només pugui posar els dos valors, he creat una taula ‘tipus’ on hi ha els dos tipus, i després relaciono codi de la taula tipus amb tipus de la taula cavalls i creare un quadre de llista perquè llisti les dues opcions o poni o cavall:

Tula cavalls:
![[Pasted image 20260928123232.png]]

Taula tipus:
![[Pasted image 20260928123243.png]]

i inserixo els tipus:
![[Pasted image 20260928123254.png]]
  

2. Insereix 5 registres a la taula CAVALLS. 

Per inserir registres, creo un formulari amb la taula cavalls:

  

A continuació per poder elegir el tipus de cavall, desagrupem el camp tipus i esborrem on sortiria el text:

Esborrem aquest:

  

I inserim un quadre combinat tal com es mostra a la imatge:

  

Seleccionem la taula que volem, amb el meu cas la taula tipus:

  

I seleccionem el camp que volem que es mostrin les dades:

Per poder veure quina dada hem seleccionat, premem que si, i on volem que es desin les dades seleccionades, amb el meu cas ‘tipus’:

  

3. Crea una consulta per a veure els animals tipus 'cavall'. 

Creem la consulta amb vista disseny i seleccionem les taules cavalls i tipus per inserir els camps de la taula cavall i el camp tipus:

Al camp tipus, filtrem per cavall amb cometes tal com es mostra:

Resultat amb vista normal:

  

4. Crea una taula anomenada CAVALLERS amb els camps nom, casa, data_naixement, regió, maestria. Tingues en compte d'afegir a la taula un camp clau. I fixaràs que al camp maestria només puguis posar tres valors, 'espasa', 'arc' i 'llança'.  

Taula Cavallers:

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

Camp nom, amb tipus text, longitud 20 i entrada requerida sí:

Camp casa, amb tipus text, longitud 30 i entrada requerida sí:

Camp data, amb tipus data i entrada requerida sí:

Camp regió amb tipus text, longitud 20 i entrada requerida sí:

Camp codi maestria amb tipus enter (clau forana)

  

Talua Tipus maestria:

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

Camp tipus maestria, amb tipus text i longitud 20:

I hem d’inserir els tipus de maestries amb vista normal:

  

5. Insereix 5 registres a la taula CAVALLERS. 

Per inserir registres, creem un formulari de la taula cavallers, inserim totes les dades:

  

Tot seguit afegim un quadre de llista com hem fet anteriorment:

I seleccionem la taula maestria:

amb el camp tipus maestria:

  

I inserim els registres:

6. Crea una consulta per mostrar quins són els cavallers que tenen maestria amb arc i els anys d'experiència. La realització d'aquesta consulta provocarà la modificació de la taula CAVALLERS. 

Inserim les taules T_cavallers i T_Maestria:

  

I inserim els camps necessaris de la taula cavallers, i el camp tipus maestria:

  

On a criteri i de maestria filtrem per Arc:

Resultat:

7. Crea una taula anomenada EOREDS amb els camps DataAlta, CodiCavaller i NomMariscal. 

Camp codi, amb tipus enter i valor autonumèric, i camp clau:

Camp DataAlta amb tipus Data, entrada requerida si:

Camp CodiCavaller amb tipus Enter:

Camp NomMariscal amb tipus text, entrada requerida si i longitud 20:

  
  

8. Crea les relacions entre la taula EOREDS, CAVALLERS i CAVALLS. 

El meu raonament és el següent, com els cavallers tenen un cavall, relacionem 1 a 1, de tal manera que cada cavall estarà assignat a un cavaller, i he relacionat el codi de la taula eored a codieored de la taula cavaller de tal manera que així es puga assignar i saber de quin eored és cada cavaller:

  

9. Insereix 3 Eoreds diferents, i per a cadascun d'ells tres Cavallers. 

Creem formulari amb auxiliar i afegim els camps codi, nomeored, nommariscal i dataalta de la taula eoreds:

Afegim subformulari amb l'opció següent:

  

I afegim els camps codi, nom, casa, data, regio, maestria i anys de la taula cavallers:

Un cop això fet, crearem un quadre de llista per a la maestria del cavaller tal com havíem fet anteriorment:

I seleccionem la taula maestria:

Amb el camp tipus maestria:

Un cop fet, afegim els registres:

10. Crea un informe per llistar els cavallers agrupats baix del seu Eored i Mariscal. 

Per fer el formulari i que sortien els cavallers llistats amb el seu respectiu eored i mariscal, creem una consulta de les taules cavallers i eoreds amb els camps “nomcavaller, NomEored i nommariscal”:

  

Ara creem l'informe de la consulta amb auxiliar i seleccionem tots els camps:

  

A agrupació seleccionem els camps nomeored i nommariscal:

  

Seguidament, a opcions d’agrupació, posem els camps de la següent manera:

Resultat final:

**