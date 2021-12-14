## Bloc 2: Laravel


## Pràctica 4

 * Quan un usuari és logueja: 

    * Ha de redirigir a la mateixa pàgina.
    * El seu nom ha d'aparèixer en el botó de logout.
    * Podrà fer like o llevar-lo en els productes (cal crear una ruta per autoritzats, amb el seu mètode al control·lador)
        
```php
// per saber si un usuari ha fet un like sobre un producte
count($this->Likes->where('id',Auth::user()->id))
// per llevar el like o possar en like en un producte
$product->Likes()->detach(Auth::user()->id);
$product->Likes()->attach(Auth::user()->id);
```

## Pràctica 5

Si l'usuari és premium podrà fer un mateniment dels seus productes:

    * Caldrà afegir una camp a l'usuari amb el tipus d'usuari.
    * Hi tindreu que crear un middleware premium.
    * Fer un CRUD amb la taula productes limitat als meus productes

Encara que no és necessari crear un middleware owner perquè als usuaris tan sols
li han d'eixir els propis productes, estaria bé fer-lo per a que un premiun no puga
modificar o esborrar (a cegues) un producte que no és seu. 

## Pràctica 6 

Un usuari normal:

    * Podrà llançar ofertes per a un o varios productes.
    * Podem possar un botó que al polsar-lo ens rediriguisca a una pàgina on podem possar un preu i on vejam tots el productes als que hem llançat ofertes.

Un usuari premium:

    * Podrà vore les ofertes rebudes i processar-les.

