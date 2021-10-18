## Bloc 1: PHP UT.5: Connexió a BD

## Exercicis curts

Crea el següents programes:

### Exercici 4.A 

* Crea la classe Connection per establir una connexió a la BD mitjançant el mètode static make, que accepta com a paràmetre la BD a connectar.
* Desde phpmyadmin.dwes.my i en la BD test:
    * Crea la taula alumnes amb els següents camps:
        * dni (primaryKey CHAR(10))
        * Nom (required CHAR(100))
        * Data de naixement (required date)
        * Sexe (CHAR (1)) DEFAULT ''
        * Hobby (smallInt) DEFAULT 0
        * Foto (CHAR(100)) NULLABLE
    *  Donam d'alta 3 alumnes       
* Crea una classe queryBuilder i fes un mètode que torne tots els elements d'una taula
* Mostra tots els alumnes en una taula

### Exercici 4.B    

* Fes que el formulari creat en un exercici anterior inserte un element en la taula.

### Exercici 4.C

* Crea dos dels següents mètodes en la classe queryBuilder i prova-los:
    * findById(nomTaula,id)
    * selectWhere(nomTaula,nomcamp,value)
    * insert(nomtaula,Array camps)
    * update(nomtaula,Array camps,id)
    * deleteById(nomTaula,id)

# Exercici 4.B

* Fes un crud d'esta taula
    * Una taula per a mostrar tots els elements
    * Amb un botó per a inserir uno nou
    * Amb dos icones per linea per tal de modificar i eliminar element de la taula.
    * Amb una icona per a vore més dades de cada element.
		
