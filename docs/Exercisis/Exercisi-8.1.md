### Exercisis

#### Exercisi 1 

Sobre el projecte batoipop , afegirem aquests canvis:

* Afig dos controlador de tipus api anomenats OfferController i ProductController

* Emplena els mètodes index , show , store , update i destroy perquè, respectivament,facen el següent:

    * index haurà de retornar en format JSON el llistat de tots els registres, amb un codi 200
    * show haurà de retornar la informació del registre que rep, amb un codi 200
    * store haurà d'inserir un nou registre amb les dades rebudes, amb un codi 201, i utilitzant el
      els validadors  que ja vas fer. 
    * put haurà de modificar els camps del registre rebuts, amb un codi 200.
    * destroy haurà d'eliminar el post rebut, retornant null amb un codi 204

Crea una col·lecció en Postman anomenada BatoiPop que definisca una petició per a cadascun dels cinc
serveis implementats. Comprova que funcionen correctament i exporta la col·lecció a un
arxiu.

### Exercisi 2

Crea una api Resource per a tornar les dades del dos controladors anteriors.

#### Exercisi 3

Com a últim exercici d'aquesta sessió, es demana que seguiu els passos indicats per protegir aplicacions basades en serveis REST mitjançant laravel Sanctum.
