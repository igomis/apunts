## Bloc 1: PHP UT.7: Laravel

## Exercicis curts


#### Exercisi 1 (Branca v2.1):
 
Sobre el projecte blog de la sessió anterior, afegirem aquests canvis:
	
* Crea una base de dades anomenada blog en el teu servidor de bases de dades a través de **phpMyAdmin**.
Modifica també l'arxiu **.env** del projecte per a accedir a aquesta base de dades amb les
credencials adequades.

* Elimina les migracions relatives a password_resets i failed_jobs, i edita la migració de la taula
users per a deixar-la únicament amb els camps login i
password, a més de l'id i els timestamps.

* Crea una nova migració anomenada crear_taula_posts , que crearà una taula anomenada posts amb aquests camps:

	* **Id** autonumérico
	* **Titol** del post (string )
	* **Contingut** del post (text )
	* **Timestamps** per a gestionar automàticament la data de creació o modificació del post

* Llança les migracions i comprova que es creen les taules corresponents amb els camps associats
en la base de dades.	

#### Exercisi 2 (Branca v2.2):

Continuem amb el projecte blog anterior. Crea un nou [model](#definició-dun-model-de-dades) anomenat Post per als posts del nostre blog. Ha de quedar  juntament amb el model d'Usuari a la subcarpeta **App\Models** del projecte.
Després, modifica els mètodes del controlador **PostController** creat en exercisis anteriors, d'aquesta manera:

* El [mètode](#consultar-dades) **index** ha d'obtindre tots els posts de la taula, i mostrar la vista **posts.index** amb aqueix llistat de posts.
	* La vista posts.index , per part seua, rebrà el llistat de posts i mostrarà els títols de cadascun, i un botó Veure per a mostrar la seua fitxa ( posts.show ).
	* Has de mostrar el llistat de posts ordenat per títol en ordre ascendent, i [paginat](#paginaciò-de-resultats) de 5 en 5.
* El [mètode](#mostrar-dades) **show** ha d'obtindre el post que el seu id es passarà com a paràmetre, i mostrar-lo en la vista posts.show .
	* La vista posts.show rebrà l'objecte amb el post a mostrar, i mostrarem el títol,contingut i data de creació del post, amb el format que vulgues.
* El [mètode](#esborrar-dades) **destroy** eliminarà el post que el seu id rebrà com a paràmetre, i retornarà la vista posts.index amb el llistat actualitzat. Per a provar aquest mètode, recorda que has de definir un formulari en una vista (el pots fer per a cada post mostrat en la vista posts.index ) que envie a la ruta posts.destroy usant un mètode [DELETE](#sobre-lesborrat-des-de-les-vistes), com hem explicat en un exemple anterior.
* Els mètodes **create , edit , store i update** de moment els deixarem sense fer, fins que vegem com gestionar formularis.
 
* Per a simular la inserció i la modificació, crearem dos mètodes addicionals en el controlador, que usarem de manera temporal:
	* Un mètode anomenat **nuevoPrueba** , que cada vegada que el cridem crearà un post amb un títol a l'atzar (per exemple, "Títol X", sent X un enter aleatori), i un contingut a l'atzar ("Contingut
X"). Pots emprar la funció **rand** de PHP per a generar aquests números aleatoris per a títol i contingut.
	* Un mètode anomenat **editarPrueba** , que rebrà com a paràmetre un id i modificarà el títol i contingut del post altres generats aleatòriament, com en el punt anterior.
	* Aquests dos mètodes (especialment el primer) ens serviran per a crear una sèrie de posts de prova que després ens serviran per a provar el llistat i la fitxa dels posts.

* En l'arxiu **routes/web.php** , recorda afegir dues noves rutes temporals de tipus **get** per a provar aquestes insercions i modificacions. La primera pot apuntar a **/movies/nuevoPrueba** ,per exemple, i la segona a **/movies/editarPrueba/{id}** . Recorda també eliminar o editar la restricció **only** de les rutes del controlador que vas establir la sessió anterior, perquè no sols permeta les rutes **index, show, create i edit**, i a més permeta la de destroy (o totes les possibles, si vols, ja que tard o d'hora les utilitzarem).


#### Exercisi 3 (Branca v2.3):

Sobre el projecte blog de la sessió anterior, afegirem aquests canvis:

* Crea una [relació](#un-a-molts) un a molts entre el model d'Usuari i el model de Post , tots dos ja existents en l'aplicació, de manera que un post és d'un usuari, i un usuari pot tindre molts posts. Hauràs de definir una nova [migració de modificació](4.5.Laravel.md#crear-una-nova-migració) sobre la taula posts que afija un nou camp usuari_id , i establir a partir d'ell la relació.
* Crea des de phpMyAdmin una sèrie d'usuaris de prova en la taula usuaris, i associa alguns d'ells als posts que hi haja.

* Modifica la vista posts/index.blade.php perquè, al costat del títol de cada post, entre parèntesi, aparega el **login** de l'usuari que el va crear.

#### Exercisi 4 (Branca v2.4):

Continuem amb el projecte blog anterior. Ara afegirem el següent:

* Crea un [seeder](#los-seeds) anomenat **UsuariosSeeder** , amb un factory associat anomenat **UsuarioFactory** (canvia de nom el que ve per defecte **UserFactory** per a aprofitar-ho). Crea amb això 3 usuaris de prova,
amb logins que siguen únics i d'una sola [paraula](#els-factories) (usa el faker), i passwords també d'una sola paraula, sense encriptar (per a poder-los identificar després, arribat el cas).
* Crea un altre seeder anomenat **PostsSeeder** amb un factory associat anomenat **PostFactory** . En el factory, defineix amb el faker títols aleatoris (frases) i continguts aleatoris (textos llargs). Usa el seeder per a crear 3 posts per a cadascun dels usuaris existents.
* Utilitza l'opció **php artisan migrate:fresh --seed** per a esborrar tot contingut previ i poblar la base de dades amb aquests nous elements. Comprova després des de la pàgina del llistat de posts, i des de phpmyAdmin que la informació és correcta.

#### Exercisi 5 (Branca v2.5):

Afig al projecte blog un nou model anomenat Comentari , juntament amb la seua migració i controlador associats. Cada comentari tindrà com a camp el contingut del comentari, i estarà relacionat un a molts amb el model Usuari , de manera que un usuari pot tindre molts comentaris, i cada comentari pertany a un usuari. També tindrà una relació un a molts amb el model Post , de manera que un comentari pertany a un post, i un post pot tindre molts comentaris. Per tant, la
migració dels comentaris haurà de tindre com a camps addicionals la relació amb l'usuari ( usuario_id ) i amb el post al qual pertany ( post_id ).

Aplica la migració per a reflectir la nova taula en la base de dades, i utilitza un **seeder** i un **factory** per a crear 3 comentaris en cada post, amb l'usuari que siga. A l'hora d'aplicar tot això, esborra els continguts previs de la base de dades ( **migrate:fresh --seed ).

AJUDA: si vols triar un usuari a l'atzar com a autor de cada comentari, pots fer una cosa així:

	Usuario::inRandomOrder()->first();


En aquest cas, seria convenient que aqueix usuari aleatori s'afija directament en el factory del comentari, i no en el seeder, ja que en cas contrari és possible que genere el mateix usuari per a tots els comentaris d'un post.
En la fitxa dels posts (vista posts/show.blade.php ), afig el codi necessari per a mostrar el **login** de l'usuari que ha fet el post, i el llistat de comentaris associat al post, mostrant per a cadascun el login de l'usuari que el va fer, i el text del comentari en si. Utilitza també la [llibreria)(us-de-dates) **Carbon** per a mostrar la data de creació del post (o la dels comentaris, com preferisques) en format d/m/Y .


