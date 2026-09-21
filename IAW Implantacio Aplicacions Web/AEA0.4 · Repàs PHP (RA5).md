
Requisits de compleció

**Oberta el** dijous, 6 de novembre 2025, 00:00

**Venciment:** divendres, 25 de setembre 2026, 23:59

### 1. Interpolació de Cometes

PHP

<?php

$nom = "Jordi";

echo 'Benvingut, $nom!\n';

echo "Benvingut, $nom!\n";

?>

Preguntes:

1. Què imprimirà la primera línia (echo '...')? Benvingut, $nom!\n
    
2. Què imprimirà la segona línia (echo "...")? Benvingut, Jordi!
    
3. Explica breument per què són diferents: Perquè les cometes simples no interpreten variables ni `\n`, mentre que les dobles sí.
    

---

### 2. Mal·leabilitat de Tipus (Type Juggling)

PHP

<?php

echo "10" + 5;         // Línia 1

echo "\n";

echo "10" . 5;         // Línia 2

echo "\n";

echo "10 gossos" + 5;  // Línia 3

echo "\n";

echo 10 + 5 . " gossos"; // Línia 4

?>

Preguntes:

1. Què imprimirà la Línia 1? 15
    
2. Què imprimirà la Línia 2? 105
    
3. Què imprimirà la Línia 3 i per què? Depèn de la versió de PHP. En versions modernes de PHP provoca un **TypeError**, perquè `"10 gossos"` no és una cadena numèrica vàlida per sumar-la a `5`.
    
4. Què imprimirà la Línia 4? 15 gossos
    

---

### 3. Àmbit de Variables (Scope)

PHP

<?php

$nom_global = "Anna";

function saludar() {

    echo "Hola, " . $nom_global;

}

saludar();

?>

Preguntes:

1. Quina és la sortida exacta d'aquest script? (Pensa en els "Notices" o "Warnings"). 
    
2. Per què la funció no pot "veure" la variable $nom_global? Perquè les variables definides fora d'una funció no són accessibles directament dins d'ella.
    
3. Escriu dues maneres diferents d'arreglar-ho. Es pot solucionar utilitzant:

```
global $nom_global;
```

O passant-la com a paràmetre:

```
function saludar($nom_global)
```
    

---

### 4. Bucles foreach (Clau i Valor)

PHP

<?php

$inventari = [

    "pomes" => 5,

    "peres" => 10,

    "taronges" => 0

];

foreach ($inventari as $fruita => $quantitat) {

    echo "Queden $quantitat de $fruita.\n";

    if ($quantitat == 0) {

        echo "CAL REPOSAR: $fruita!\n";

    }

}

?>

Preguntes:

1. Què conté la variable $fruita a la primera iteració del bucle? $fruita conte "pomes".
    
2. Què conté la variable $quantitat a la primera iteració del bucle? 5
    
3. Escriu la sortida completa i exacta que produirà aquest script. 
Queden 5 de pomes.
Queden 10 de peres.
Queden 0 de taronges.
CAL REPOSAR: taronges!
    

---

### 5. Superglobals ($_GET i $_POST)

Context: Un usuari envia un formulari (method="POST") que va a la URL: processar.php?id=123.

El formulari contenia un camp <input type="text" name="usuari" value="Carles">.

Codi (processar.php):

PHP

<?php

$id_url = $_GET['id'];

$nom_formulari = $_POST['usuari'];

$id_formulari = $_POST['id']; // Compte aquí

echo "ID URL: $id_url\n";

echo "Nom Formulari: $nom_formulari\n";

echo "ID Formulari: $id_formulari\n";

?>

Preguntes:

1. Què imprimirà la línia "ID URL:"? 123
    
2. Què imprimirà la línia "Nom Formulari:"? carles
    
3. Què passarà a la línia "ID Formulari:"? Per què?
    

---

### 6. include vs. require

Context: El fitxer config.php no existeix al servidor.

Codi A:

PHP

<?php

echo "Inici Codi A\n";

include 'config.php';

echo "Final Codi A\n";

?>

Codi B:

PHP

<?php

echo "Inici Codi B\n";

require 'config.php';

echo "Final Codi B\n";

?>

Preguntes:

1. Quina serà la sortida completa del Codi A? (Què es veurà a la pantalla?)
    
2. Quina serà la sortida completa del Codi B?
    
3. Quina és la diferència fonamental entre include i require quan un fitxer falla?
    

---

### 7. Arrays: Còpia vs. Referència

PHP

<?php

$array_a = ["a", "b", "c"];

// Assignació per valor (còpia)

$array_b = $array_a;

$array_b[0] = "X";

// Assignació per referència

$array_c = &$array_a;

$array_c[1] = "Y";

print_r($array_a);

print_r($array_b);

print_r($array_c);

?>

Preguntes:

1. Quina serà la sortida de print_r($array_a)?
    
2. Quina serà la sortida de print_r($array_b)?
    
3. Explica per què $array_a ha canviat en modificar $array_c, però no en modificar $array_b.
    

---

### 8. El Parany del $this (OOP)

PHP

<?php

class Usuari {

    public $nom = "Anònim";

    public function getNom() {

        // Error comú aquí

        return $nom;

    }

    public function setNom($nou_nom) {

        $this->nom = $nou_nom;

    }

}

$u = new Usuari();

$u->setNom("Elsa");

echo $u->getNom();

?>

Preguntes:

1. Quina és la sortida exacta d'aquest script?
    
2. Per què no imprimeix "Elsa"?
    
3. Com s'arregla la funció getNom()?
    

---

### 9. Propietats static vs. Instància (OOP)

PHP

<?php

class Comptador {

    public static $total_instancies = 0;

    public $comptador_propi = 0;

    public function __construct() {

        self::$total_instancies++;

        $this->comptador_propi++;

    }

}

$c1 = new Comptador();

$c2 = new Comptador();

$c3 = new Comptador();

$c3->comptador_propi = 5;

echo "Total instàncies: " . Comptador::$total_instancies . "\n";

echo "Comptador C1: " . $c1->comptador_propi . "\n";

echo "Comptador C3: " . $c3->comptador_propi . "\n";

?>

Preguntes:

1. Què imprimirà "Total instàncies:"?
    
2. Què imprimirà "Comptador C1:"?
    
3. Què imprimirà "Comptador C3:"?
    
4. Explica la diferència entre $total_instancies (static) i $comptador_propi (no static).
    

---

### 10. Visibilitat (Public, Protected, Private) (OOP)

PHP

<?php

class Animal {

    private $nom_privat = "ANIMAL";

    protected $edat_protegida = 10;

    public $color_public = "Marró";

    public function getNomPrivat() {

        return $this->nom_privat;

    }

}

class Gos extends Animal {

    public function testAccedir() {

        echo $this->nom_privat;     // Línia A

        echo $this->edat_protegida; // Línia B

        echo $this->color_public;   // Línia C

    }

}

$g = new Gos();

echo $g->getNomPrivat(); // Línia D

echo $g->edat_protegida; // Línia E

$g->testAccedir();       // Línia F

?>

Preguntes:

1. Línia D: Funcionarà? Què imprimirà?
    
2. Línia E: Funcionarà? Per què sí o per què no?