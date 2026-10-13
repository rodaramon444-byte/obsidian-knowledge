## **Introducció**

El rei Théoden, conscient de la importància dels cavalls per a la glòria de Rohan, ha decidit establir una clínica veterinària a Edoras per assegurar la salut i el rendiment dels seus corsers i sementals. Els Rohirrim, coneguts com a Senyors dels Cavalls, necessiten un sistema que permeti portar un control exhaustiu dels mateixos. L’objectiu del sistema és garantir la traçabilitat sanitària dels cavalls i optimitzar els recursos de la clínica.

## **Document ERS**

A Edoras, capital de Rohan, els **Rohirrim han establert una clínica veterinària** per cuidar els seus cavalls de guerra i de viatge. Els **cavalls són vitals per la societat rohirrim**, i es vol portar un registre complet de la seva salut i dels serveis que presten. És per això que ens interessa emmagatzemar el seu nom, la raça, l’edat, el color, el pes i l’estat en que es troben, que pot ser apte, entrenament o recuperació.

**Cada cavall pertany a un propietari responsable**, però pot canviar de mans al llarg del temps per herència, venda o recompenses. Els propietaris solen ser membres de famílies nobles o guerrers, i a vegades comparteixen cavalls per a expedicions o missions especials. Dels propietaris ens interessa saber el seu nom, cognom, llinatge, rang militar, ciutat on viu i zona de Rohan d’on provenen.

A la clínica treballen diversos **sanadors (veterinaris)** dels quals ens interessa saber el seu nom, cognom i família a la que pertanyen, cadascun d’ells té una especialitat concreta: alguns en lesions de batalla, altres en malalties o nutrició, i alguns coneixen remeis especials derivats d’herbes de Rohan o màgia curativa. Quan un cavall és atès, **pot rebre tractaments de naturalesa diversa**, els tipus poden ser els següents, ferida, malaltia, nutrició i màgia, i en alguns casos diversos sanadors participen en el mateix procés. Ens interessa saber la data d’inici del tractament i la data de fi dels mateix, així com el resultat del mateix, que pot ser satisfactori o insatisfactori.

Sovint, els tractaments inclouen l’administració de **receptes especials o medicaments per ajudar el cavall a recuperar-se**, ens interessa saber el seu nom, una breu descripció, el tipus de medicament, que pot ser un ungüent, un beuratge, pols màgica o herba medicinal, la dosis recomanada i els efectes secundaris. La **dosi i la durada** dels medicaments s’adapten a cada cavall i al tractament que rep.

Els **cavalls també poden ser sol·licitats** per habitants o aliats de Rohan **per prestar serveis al llarg del temps**: transport, participació en cerimònies, missatgeries urgents, etc. Aquests serveis externs es fan sempre sota la **supervisió dels propietaris** i amb la intervenció de la clínica, que pot cobrar una comissió en funció del tipus d’encàrrec.

 El sistema ha de permetre registrar **quins cavalls hi ha**, qui en són **els propietaris** al llarg del temps, quins tractaments han rebut i amb quin resultat, quins veterinaris hi han intervingut, quins medicaments s’han administrat en cada tractament i amb quina dosi, i també quins **serveis** han prestat fora de la clínica amb la corresponent supervisió i comissió.

==🔴Preguntes guia==

- Quin tipus d’informació sobre els medicaments caldria emmagatzemar? 
- Pot un tractament incloure més d’un medicament? 
- I poden participar diversos veterinaris en el mateix tractament? 
- Com relacionaries un medicament amb el cavall, el tractament i el veterinari?
- Els serveis externs impliquen només cavall i client, o també propietari i clínica? 
- Hi ha informació que pertany més a una relació que a una entitat?

## **Tasques a realitzar**

1. Realitza una llista de candidats a entitats, relacions i atributs, indicant després si l'admets o la refuses com a tal indicant el perquè de la teua decisió. Per a cada relació, raona el seu tipus i la seva cardinalitat.


| ENTITATS           | ATRIBUTS                                                                                                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------- |
| CAVALL             | - Codi    <br>- Nom    <br>- Raça    <br>- Edat    <br>- Color    <br>- Pes    <br>- Estat                 |
| PROPIETARI         | - Codi    <br>- Nom    <br>- Cognom    <br>- Llinatge    <br>- Rang militar    <br>- Ciutat     <br>- Zona |
| SANADOR            | - Codi    <br>- Nom    <br>- Cognom    <br>- Família    <br>- Especialitat                                 |
| TRACTAMENTS        | - Codi    <br>- Tipus    <br>- DataInici    <br>- DataFi    <br>- Resultat                                 |
| MEDICAMENT (feble) | - Codi    <br>- Nom    <br>- Descripcio    <br>- Tipus    <br>- Dosis    <br>- Efectes secundaris          |
| SERVEIS            | - Codi    <br>- CodiCavall    <br>- DataInici    <br>- DataFi                                              |

| RELACIONS                                                      | ATRIBUTS                                                |
| -------------------------------------------------------------- | ------------------------------------------------------- |
| Cavall-Propietari “pertànyer”<br><br>Cardinalitat: 1:N         | —                                                       |
| Cavall-Tractament “Rep_tractament”<br><br>Cardinalitat: 1:N    | - Data inici<br>    <br>- Data fi<br>    <br>- resultat |
| Sanador - Tractament “Participa”<br><br>Cardinalitat: N:M      | —                                                       |
| Tractament - Medicament “Administrar”<br><br>Cardinalitat: N:M | - Dosi<br>    <br>- Durada                              |
| Cavall - Servei “Solicitar”<br><br>Cardinalitat: 1:N           | —                                                       |
| Propietari - Servei “Supervisar”<br><br>Cardinalitat 1:N       | —                                                       |


Grup 