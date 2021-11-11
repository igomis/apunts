## Bloc 1: Examen 

Coses a fer:

1. Borra el directori examen i fes un git del repostori de la manera: **git clone nom_repositori examen** estan en el directori code. 
2. Crea una BD employees i importa el fitxer **employees.sql**. Estableix la connexió amb la BD employees i prepara la web per a poder vore-ho.
3. En el queryBuilder crea la funció **find(taula,primaryKey,id)** per a buscar en una taula per clau primaria **(0,5p)**

4. Completa la classe employees amb el mètodes:
	* getDepartment() 	**(0,5p)**   Nom del departament actual de l'empleat
	* getTitle()		  	**(0,5p)**  Nom del title actual de l'empleat
	* isManager()			**(0,5p)** Es manager de un departament o no
	* Managers() **(0,5p)** Array d'objectes employees amb els managers de l'empresa (no managers de departament)

5. Modifica la vista **employees.view.php** per a mostrar un empleat. **(0,5p)**

6. Modificat la vista **menu.view.php** per a mostrar tots els departaments.**(0,5p)** L'enllaç que al que ha de redirigir en cada departament serà **index.php?dep=claudep** **(0,5p)**. Al polsar en un departament els empleats de le pàgina principal canviaran i es mostraran els de eixe departament.
 Hi ha un metode en Employee::Members(dept) que trau tots els empleats del departament. 
 **(0,5p)**. Si no hi ha cap departament seleccionat es mostraran els managers.

6. Per a identificar a un usuari s'utilitza el numero d'empleat i el nom. Si no coincidix no es farà res. Si coincidix en el peu eixirà el nom de l'usuari i un botó per a fer logout **(0,75p)**. El logout.php ha de funcionar **(0,25p)**

Pots triar una de le següents coses:

7. Els emplats poden votar l'empleat de l'any. Han de votar a un membre del seu departament, a un de fora i a un manager **(1p)** i s'incrementarà el camp votes. Soles pot votar una volta (hi ha un camp vote en la taula employees per a saver si ha votat) **(0.5p)**. Les possibles errades es control·laran per exempcions que rediguiran a un pàgina amb l'error produït i ammb un enllaç per tornar a la pàgina principal. **(1p)**.
Es mostrarà en el peu de pàgina els vots que ha fet l'usuari: nom,departament,title **(1p)**. Es podran esborrar els vots actuals i tornar a votar **(1p)**.
 
8. Els managers tindran un enllaç en el peu (on ara posa vots actuals i que redigirà a una pàgina on hi haura un crud de la taula employees) 
	* Es mostrarà una taula amb els empleats del seu departament **(0,5p)**
	* Es pot donar d'alta un empleat **(1p)**
	* Es pot donar de baixa un empleat. No es borarrà, canviarà la data corresponent. **(0,5p)**
	* Es pot modificar un empleat. Si un empleat es canvia de departament, no es donarà de baixa la relació sino que s'afegiran les dades de finalització en un departament i es crearà la relació de l'altre.**(1,5p)**
	* Es validaran els camps mostrant els errors en el formulari i mantenint els camps originals **(1p)**
	 
 	  	 

