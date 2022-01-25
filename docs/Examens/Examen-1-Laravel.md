## Bloc 2 i 3: Examen de prova

Ganga Severa:

* Crea un nou repositori per al projecte
* Configura el .gitignore per a no incloure en el repositori els següents arxius i carpetes:
   * carpeta vendor i node_modules
   * arxius .*env 
* Partin de la plantilla sumministrada s'han de fer algunes coses:

Base de dades i models (2p)

* Crea la base de dades ganga_severa.
* Modifica el .env per a poder connectar-te a ella.
* Configura la base de dades amb migracions, seeder i factories , oblida't d'usar la consola mysql 
* La taula Ganga ha de contindre les següents columnes (0,5p):
     * id únic i autoincremental
     * title: un títol per a la ganga
     * description: descripcion de la ganga
     * url: un camp per a introduir la URL externa de la ganga
     * id_category: albergarà la categoria de les gangues
     * points: un nombre enter que indique la puntuació de la ganga
     * price: preu per a albergar el preu de la ganga
     * discount_price: preu descompte per a albergar el nou preu
     * available: disponible de tipus boolean
* Hauràs de crear una taula de categories (0,25p):
     * id unic i autoincremental
     * title: un titol per a la categoria
* Haurà d'afegir un camp admin als usuaris per saver si son administradors. (0,25p)
* Has de crear 60 ganges,3 categories i 5 usuaris (1 administrador). (0,5p)
* S'han de crear els models i les relacions (0,5p)

Imatges (0,5p)

* Cada ganga ha de contindre una imatge que estarà guardada en public/img
* El nom de les imatges ha d'estar composta per la següent fórmula idGanga-ganga-severa.extensio
Per exemple: 25-ganga-severa.jpg
* Les imatges les pots possar al principi automàticament encara que siga la mateixa 

Plantilla i Pàgina principal (2,5p)

* Elements estàtics. Hi ha uns certs elements que sempre es mostren en totes les vistes del lloc web. A continuació es llisten els elements que han d'estar si o si en totes les plantilles que creeu. (0,5p)
  * Barra de dalt
  * Menu
  * Peu de pàgina
* El login ha de redirigir a la pàgina de login de laravel 
* Crea un fitxer de configuració on posses les dades del teu contacte del lloc web i fes que isquen en compte de les que hi ha (0,25p)
* Fes que el copyright l'agafe de la data actual (0,25p)
* Quan l'usuari es administrador ix la part del menú per a crear noves ganges.(0,25p)
* El nom de les categories han de substituir als que hi ha per defecte (0,25p)
* Per construir la pàgina principal has de crear totes les fitxes del producte. (has de tindre en compte que primer es creen les d'una categoria i així sucesivament). Una forma de fer-ho és utilitzar els agrupaments
en una collecció de laravel (groupBy)[https://laravel.com/docs/8.x/collections#method-groupby]) (1p)

Pagina de contacte (0,25p)

* Crea pàgina de contacte amb la plantilla i les dades de contacte del fitxer de configuració creat dalt.

Pàgina de Ganga (0,75p)

* Quan punxem en un de les gangues del llistats hem de ser redirigits a aquesta vista on podrem veure tota la informació del taula ganga. Pots maquetar-la com vulgues. (0,5p)
* Cada ganga ha de contindre els seus botons d'editar i esborrar que faça les funcions que toquen quan l'usuari autenticat tinga permisos per a fer-ho. Pots utilitzar icones per a cadascun dels botons. (0,25p)

Crear i Modificar Nova Ganga (1,5p)

* Formularis i mètodes en el control·lador. (0,5p)
* Soles pot fer-ho l'administrador (0,5p). S'han de validar el camps (tots són requerits, el discount price ha de ser menor que el price, la categoria ha d'existir) (0,5p)

Pàgina d'ofertes (0,5p)

* Fes una pàgina d'ofertes per als usuaris loguejats on isquen els productes amb més descompte.

API (2p)

* Crear un apiGangaController per a que funcionen les rutes bàsiques de la ganga (no cal autenticació) (0,25p cada ruta) 
* Validació en el servidor (0,25p) igual que dalt.
* Torna missatge d'error en json (0,25p)
* Autenticació per a post, put i delete (0,25p)

 
  	 

