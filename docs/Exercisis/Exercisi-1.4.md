## Bloc 1: PHP UT.2: Introducció

## Exercisis curts

Crea un programa php **funcions.php**  i resol els següents exercicis :

### Exercisi 1.F

Crea un funcio php contrasenyaSegura() que retorne si contraseña és o no segura. Comprovarà:

* que la seua longitud siga major o igual de 8 caràcters
* que conté alguna majúscula
* que conté alguna minúscula
* que conté algun número
* que conté algun d'aquests caràcters: guió, barra baixa, igual, asterisc, mes, dòlar, arrova o coixinet


### Exercisi 1.G

Crea una funció anomenada insert() que ens genere una sentència **insert into** en sql. Per a açò la funció rebrà dos paràmetres: El nom de la
taula i un array associatiu que contindrà els noms i valors dels
camps de la taula. La sentència resultant tindrà la següent forma:
"**insert into nom_taula (noms camps separats per comes) values (valors separats per comes amb comilles)**". 

**Ajuda**: utilitza les funcions **sprintf**, **implode** i **array_keys**.

### Exercisi 1.H

Modifica la calculadora per a que es puga passar un tercer paràmetre amb l'operació a realitzar.

* l'operació sera un string que pot valdre ('suma','resta','multiplicacio','divisio')
* si el paràmetre no està o no val una d'estes coses se li recordarà a l'usuari de que el pose.
* la funció per a calcular el resultat tindrà tres paràmetres, el primer serà una funció.
* hauràs de fer una funció per operació.


