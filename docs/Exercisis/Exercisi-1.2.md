## Bloc 1: PHP UT.2: Introducció

## Exercisis curts

### Exercisi 1.C

* Escriu un programa (cadenes.php) que accepte per la direcció un paràmetre anomenat nom i:

* Mostra-lo eliminant els caràcters '/' del principi i el final si els hi haguera (funció trim). 
* En cas de que el paràmetre nom no estiga definit utilitza el teu nom de pila.
* Mostra la longitud del paràmetre nom (funció strlen)
* Mostra el nom en majúscules (funció strtoupper)
* Mostra el nom en minúscules (funció strtolower)
* Mostra el nom substituint les lletres ‘o’ pel número zero (siga majúscula o minúscula) (funció str_ireplace).
* Mostra el nombre de vegades que apareix la lletra a (majúscula o minúscula)(funcions substr_count + (strtoupper o strtolower)).
* Mostra la posició de la primera a existent en el nom (siga majúscula o minúscula). Si no hi ha cap mostrarà un text indicant-ho (funció stripos).


Passa un segon paràmetre per querystring anomenat prefix (per a passar més d'un paràmetre per la querystring has de separar-los utilitzant el caràcter &). Després:
 
* Indica si el nom comença pel prefix passat o no (funció strpos), es distingirà entre majúscules i minúscules. 
* Si no es passa el prefix no es realitzarà aquesta operació.

### Exercisi 1.D

La funció parse_url ens permet extraure diferents parts d'una url. A partir d'una variable que continga una url, per exemple: $url = 'http://username:password@hostname:9090/path?arg=value'; Utilitza la funció parse_url per a extraure i mostrar per pantalla les següents parts de la variable url de l'exemple:

* El querystring de la url (en l'exemple arg=value)
* El protocol utilitzat (en l'exemple http).
* El nom d'usuari (en l'exemple username).
* El path de la url (en l'exemple /path)


