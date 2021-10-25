## Bloc 1: PHP UT.6: Seguretat en PHP

## Exercicis curts

Crea el següents programes:

### Exercici 6.A 

* Afig un fitxer .env al teu programa i posa els paràmetres de connexió a la BD en ell. 
* Instal·la el paquet per a poder importar-los i canvia la classe connectionnper a que els utilitze.

### Exercici 6.B    

* Crea el lloc de l'ofegat.
* Les paraules s'agafaran d'una taula de paraules.
* L'usuari només pot triar una vocal.
* Els intents invàlids permesos són 6.(O F E G A T) 
* Cada vegada se li ha de mostrar a l'usuari els intents que li queden ( de vocals i consonants) i les que lletres que ja ha dit

Pots utilitzar la següent classe

 ```php
<?php


namespace App;


class Ofegat
{
    protected $paraula;
    protected $vocal;
    protected $letters;

    /**
     * Ofegat constructor.
     * @param $paraula
     * @param $invalidsPermesos
     */
    public function __construct($paraula)
    {
        $this->paraula = strtoupper($paraula);
        $this->vocal = 0;
        $this->letters = [];
    }

    public function addLetter(String $letter)
    {
        $letter = strtoupper($letter);
        if (self::isVocal($letter)){
            if ($this->vocal == 0){
                $this->vocal = 1;
                $this->letters[] = $letter;
            }
            else {
                throw new \Exception('Ja has triat més vocals anteriorment');
            }
        } else {
            if (in_array($letter,$this->letters)) throw new \Exception('Ja la has ficada abans');
            $this->letters[] = $letter;
        }
        return (strpos($this->paraula,$letter)===false)?1:0;
    }

    public function render(){
        for($i=0;$i<strlen($this->paraula);$i++){
            if (in_array($this->paraula[$i],$this->letters)) {
                echo $this->paraula[$i];
            }
            else {
                echo "_";
            }
            echo " ";
        }
    }

    public static function isVocal($letter){
        return (in_array($letter,['A','E','I','O','U']));
    }

}
 ```
 
 El funcionament és :
 require_once ('../kernel.php');
use App\Ofegat;

```php
$intendInvalids = 0;
$ofegat = new Ofegat('Indecible');
$intendInvalids += $ofegat->addLetter('i');
$ofegat->render();
$intendInvalids += $ofegat->addLetter('b');
$ofegat->render();
$intendInvalids += $ofegat->addLetter('z');
$ofegat->render();
dd($intendInvalids);
```
 

### Exercici 6.C

* Crea un login per a poder jugar. Si no estas loguejat no pots jugar.

