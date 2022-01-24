## Bloc 2 i 3: Examen de prova

Ganga Severa:

* Crea un nou repositori per al projecte
* Configura el .gitignore per a no incloure en el repositori els següents arxius i carpetes:
   * carpeta vendor i node_modules
   * arxius .*env 
* Partin de la plantilla sumministrada s'han de fer algunes coses:

Base de dades (2,5p)

* Configura la base de dades amb migracions, seeder i factories , oblida't d'usar la consola mysql 
* La taula Ganga ha de contindre les següents columnes:
     * id únic i autoincremental
     * title: un títol per a la ganga
     * description: descripcion de la ganga
     * url: un camp per a introduir la URL externa de la ganga
     * id_category: albergarà la categoria de les gangues
     * points: un nombre enter que indique la puntuació de la ganga
     * price: preu per a albergar el preu de la ganga
     * discount_price: preu descompte per a albergar el nou preu
     * available: disponible de tipus boolean
* Hauràs de crear una taula de categories:
     * id unic i autoincremental
     * title: un titol per a la categoria
* Haurà d'afegir un camp admin als usuaris per saver si son administradors.
* Has de crear 60 ganges,3 categories i 5 usuaris (1 administrador).
* Cada ganga ha de contindre una imatge que estarà guardada en public/img
* El nom de les imatges ha d'estar composta per la següent fórmula idGanga-ganga-severa.extensio
Per exemple: 25-ganga-severa.jpg
* Les imatges les pots possar al principi automàticament encara que siga la mateixa 

Pàgina principal i vore ganga

* El login ha de redirigir a la pàgina de login de laravel (0,25p)
* El nom de les categories han de substituir als que hi ha per defecte (0,25p)
* Crea un fitxer de configuració on posses les dades del teu contacte del lloc web i fes que isquen en compte de les que hi ha (0,25p)
* Fes que el copyright l'agafe de la data actual (0,25p)
* Quan l'usuari es administrador ix la part del menú per a crear noves ganges.(0,5p)
* Elements estàtics. Hi ha uns certs elements que sempre es mostren en totes les vistes del lloc web. A continuació es llisten els elements que han d'estar si o si en totes les plantilles que creeu. (1p)
   * Barra de dalt
   * Menu
   * Peu de pàgina
* Per construir la pàgina principal has de crear totes les fitxes del producte. (has de tindre en compte que primer es creen les d'una categoria i així sucesivament). Una forma de fer-ho és utilitzar els agrupaments
en una collecció de laravel (groupBy)[https://laravel.com/docs/8.x/collections#method-groupby]) (1p)

Pagina de contacte

* Crea pàgina de contacte amb la plantilla i les dades de contacte (0,5p)

Pàgina de Ganga 

* Quan punxem en un de les gangues del llistats hem de ser redirigits a aquesta vista on podrem veure tota la informació del taula ganga. Pots maquetar-la com vulgues. El camp disponible no és necessari que el mostres en aquesta vista (0,5p)
* Cada ganga ha de contindre els seus botons d'editar i esborrar que faça les funcions que toquen quan l'usuari autenticat tinga permisos per a fer-ho. Pots utilitzar icones per a cadascun dels botons. (0,5p)

Pàgina de crear Ganga

* Has de crear un formulari que actualitze la BD (0,5) amb validació en el servidor (0,5p). Tots els camps són requerits.

API 

* Crear un apiGangaController per a que funcionen les rutes bàsiques de la ganga (no cal autenticació) (0,3p cada ruta) 

 
  	 

