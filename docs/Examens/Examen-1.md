## Bloc 1: Examen de prova

Coses a fer:

1. Crea una BD music i importa el fitxer **music.sql**. Estableix la connexió amb la BD music **(0,25p)**

2. Modifica la funció **loadTemplate($vista,$params)** del **myHelpers** per tal que accepte la vista en el format (**directori.fitxer** en compte de **directori/fitxer**). 

	**Example**: Si vull carregar la vista **article.view.php** dins del directori **templates** ara he de fer **loadTemplate('templates/article')** i l'exercici demana que es faça de la següent manera **loadTemplate('templates.article')**. 

	Canvia la vista **index.view.php** per a adoptar eixe format. **(1p)**

3. Completa la classe albums amb el mètodes:
	* getArtistName() 	**(0,5p)**
	* getCompany		  	**(0,25p)**
	* Best()				**(0,5p)**

4. Modifica la vista **main.view.php** per a que carregue 12 vistes de **article.view.php** amb els 12 articles més puntuats. **(0,5p)**
5. Modifica la vista **article.view.php** per a mostrar l'article en qüestió. **(0,5p)**
6. Quan un usuari s'identifica:
	* Es dona d'alta en la tabla usuaris (si no existeix) **(0,75p)**
	* Es mostra el nom en compte del formulari d'identificació (footer.php) **(0,5p)**
	* Ja pot votar, o siga la pàgina votes.php queda activada. Fins eixe moment la pàgina votes.php rediriguia a index.php. **(0,25p)**
7. Modifica la vista menu per a que agafe les dades de l'array menu que està en **config/menu.php**. Has de tindre en compte que una opció del menu eixirà depenent de l'opció auth de l'array i de si l'usuari està identificat o no. Un auth 1 en un opció del menú sera visible quan l'usuari estiga identificat i el contrari en la 0. **(1p)**
8. Un usuari pot votar només 3 vegades **(1p)**. Ho farà polsant sobre una imatge (votes.php) i que cada vot incrementarà el valor del camp **votes** de la taula **albums** **(0,5p)** de manera inversament proporcional a l'order del vot que ha fet. La primera vegada 3, la segona 2 i la tercera 1. Si intenta votar més voltes se li comunicarà a l'usuari que ja ha votat tres vegades. **(0,25p si s'utilitza una exempcio)**
9. Un usuari pot desfer el seu vot amb l'opció corresponent del menú. Si l'utilitza els vots quedaran anul·lats  i s'hauran de descomptar de la BD **(1p)** (tots a l'hora **(0,25p)**). L'usuari podrà tornar a votar.
10. Crear el logout.php **(0,25p)**
11. Crea la funcion **last($taula)** **(0,75p)** en el queryBuilder per a que mostre l'ultim registre d'una taula 
 	  	 

