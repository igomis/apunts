## Bloc 1: PHP UT.7: Laravel

## Exercicis curts


#### Exercisi 1 (Branca v3.1)

Sobre el projecte blog, afegirem aquests canvis:

* Crea un [formulari](../7.7.Laravel_validacio.md#creació-i-enviament-de-formularis) per a donar d'alta nous posts, en la vista
**resources/views/posts/create.blade.php** . fig un parell de camps (un text curt i un text llarg) per a emplenar el títol i el contingut, i com a autor o usuari del post de moment deixa un predefinit; per exemple, l'autor amb id = 1, o el primer autor que trobes en la base de dades ( Autor::get()->first() ). Més endavant ja ho farem dependent de l'usuari que s'haja autenticat. Recorda definir el mètode store en el controlador de posts per a donar d'alta el post, i redirigir després al llistat principal de posts. Per a carregar el formulari, afig una nova opció en el menú principal de navegació.

* En la fitxa d'un post, afig un botó amb un formulari per a esborrar el post. Hauràs de definir el codi del mètode **destroy** per a eliminar el post i redirigir de nou al llistat. Deuràs eliminar tots els comentaris associats a aqueix post, i després esborrar el post. Per a filtrar els comentaris d'un post i esborrar-los, utilitza la clàusula **where**.

```
Comentario::where('post_id', $id)->delete();
```
#### Exercisi 2 (Branca v3.2)

Ara afegirem el [formulari d'edició](../7.7.Laravel_validacio#actualitzacions-i-esborrats) d'un post, també des de la vista de la fitxa del post. El formulari haurà de mostrar les dades ja farcides del post. Aquestformulari es carrega a partir del mètode **edit** (que haurà de renderitzar la vista amb el formulari d'edició,
**resources/views/posts/edit.blade.php** ), i el formulari s'enviarà al mètode **update** del controlador, passant-li com a paràmetre l'id del post a modificar.

#### Exercisi 3 (Branca v3.3)

Crea un **form request** anomenat **PostRequest** , que [valide](../7.7.Laravel_validacio#utilitzar-form-requests-per-a-validacions-més-complexes) les
dades del post. En concret, han de complir-se aquests requisits:

	* El títol del post ha de ser obligatori, i d'almenys 5 caràcters de longitud
	* El contingut del post ha de ser obligatori, i d'almenys 50 caràcters de longitud

Defineix [missatges d'error](../7.7.Laravel_validacio#mostrar-missatges-derror) personalitzats per a cada possible error de validació, i mostra'ls al costat de cada camp afectat. A més, utilitza la [funció **old**](../7.7.Laravel_validacio#recordar-valors-enviats) per a recordar el valor antic correcte, en el cas que un camp passe la validació però un altre(s) no.

#### Exercisi 4 (Branca v3.4)

Sobre el projecte blog de la sessió anterior, afegirem aquests canvis:

* Modifica l'arxiu **config/auth.php** perquè el provider acudisca al model correcte d'usuari.

* Modifica el factory d'usuaris perquè els passwords s'encripten amb **bcrypt**. Perquè siga fàcil de recordar,
fes que cada usuari tinga com a password el seu mateix login encriptat. Executa després
**php artisan migrate:fresh --seed** per a actualitzar tota la base de dades.

* Crea un formulari de login juntament amb un controlador associat, i les rutes pertinents per a mostrar el formulari o autenticar. Recorda cridar "login" a la ruta que mostra el formulari de login.

* En el controlador de posts, protegeix totes les opcions menys les de index i show .

* Afig una opció de Login en el menú de navegació superior, que només estiga visible si l'usuari no s'ha
autenticat encara.

* Fes que només es mostren els enllaços i botons de crear, editar o esborrar posts quan l'usuari estiga
autenticat. En aqueix mateix cas, fes que també es mostre una opció de logout en el menú
superior, que hauràs d'implementar.

* Finalment, afig la funcionalitat que l'usuari autenticat només pot editar i esborrar els seus
propis posts, però no els dels altres usuaris.

#### Exercisi 5 (Branca v3.5)

Continuem amb el projecte blog anterior. Segueix aquests passos per a definir una autenticació basada en
rols:

* Crea una nova migració que modifique la taula d'usuaris per a afegir un nou camp anomenat rol,
de tipus string. Assegura't que la migració siga de modificació, i no de creació de taula. Després,
executa-la per a crear el nou camp.

* Fes que algun dels usuaris de la taula tinga un rol de **admin** (edita'l a mà des de phpMyAdmin),
i la resta seran de tipus editor.

* Crea un nou middleware anomenat **RolCheck** , amb una funció que comprove si l'usuari té el
rol indicat, com en l'exemple vist abans en les anotacions. Registra-ho adequadament en l'arxiu
App/Http/Kernel.php , com s'ha explicat.

* Modifica les vistes necessàries perquè, si l'usuari és de tipus **admin** puga veure els botons d'edició
i esborrat de qualsevol post, encara que no siguen seus.

* Modifica els mètodes edit , update i destroy de PostController perquè redirigisquen a posts.index
si l'usuari no és administrador, o si no és el propietari del post a editar o esborrar.
 



