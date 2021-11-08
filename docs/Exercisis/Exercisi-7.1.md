## Bloc 1: PHP UT.7: Laravel

## Exercicis curts

#### Exercisi 1 (Branca v1.1)

* Baixa el projecte blog del github classroom amb git clone dins del directori code. 
* Configura-lo en el domini blog.my en la teua màquina real modificant el /etc/hosts.

* En el homestead:

	* Configura el ngingx per afegir el domini:

```
	serve blog.my /home/vagrant/code/blog/public
	sudo service nginx restart
```

* Configura el laravel 

```
	cp .env.example .env
	sudo chmod -R 777 bootstrap/cache
	sudo chmod -R 777 storage
	sudo chmod -R 777 storage/logs
	php artisan key:generate
	composer install
```

*  Prova d'accedir a la pàgina d'inici d'aquest nou projecte. Edita el fitxer routes/web.php i [afig una nova ruta a la URL posts](../7.3.Laravel_rutes_vistes.md#rutes-simples). En accedir a aquesta ruta (http://blog/posts), haurem de veure un missatge amb el text "Llistat de posts". 

* Genera la branca v1.1 i fes un commit i un push. A partir d'ara cada exercisi s'ha de pujar a la branca corresponent.

#### Exercisi 2 (Branca v1.2)

Afig una nova [ruta parametritzada](../7.3.Laravel_rutes_vistes.md#afegir-paràmetres-a-les-rutes) a **posts/{id}** , de manera que el paràmetre id siga numèric (és a dir, només continga dígits del 0 al 9) i obligatori. Fes que la ruta retorne el missatge "Fitxa del post XXXX", sent XXXX l'id que haja rebut com a paràmetre.

Posa un [nom](../7.3.Laravel_rutes_vistes.md#named-routes) a les tres rutes que hi ha definides fins ara: 

 * a la pàgina d'inici posa-li el nom "inici"
 * a la del llistat la direm "posts_llistat"
 * a la de fitxa que acabes de crear, la direm "posts_fitxa".


Definix una [plantilla](../7.3.Laravel_rutes_vistes.md#definir-plantilles-comunes) anomenada plantilla.blade.php en la carpeta de vistes del projecte ( resources/views ). Defineix una capçalera amb una secció yield per al títol, i una altra per al contingut de la pàgina, com la de l'exemple que hem vist anteriorment.
Defineix en un arxiu a part en la subcarpeta **partials** , anomenat **nav.blade.php** , una barra de navegació que ens permeta accedir a aquestes direccions de moment:

	* Pàgina d'inici
	* Llistat de posts

[Inclou la barra de navegació en la plantilla base](../7.3.Laravel_rutes_vistes.md#incloure-vistes-dins-daltres) que has definit abans
A partir de la plantilla base, [defineix altres dues vistes](../7.3.Laravel_rutes_vistes.md#definir-plantilles-comunes) en una subcarpeta posts , anomenades posts/llistat.blade.php i posts/fitxa.blade.php . Com a títol de cada pàgina posa un breu text del que són (per exemple, "Llistat posts" i "Fitxa post"), i com a contingut de moment deixa un encapçalat **h1** que indique la pàgina en la qual estem: "Llistat de posts" o "Fitxa del post XXXX", on XXXX serà [l'identificador del post](pasar-valor-a-les-vistes) que haurem passat per la URL (i que deuràs passar a la vista). 

Fes que les rutes corresponents de routes/web.php que ja has definit [renderitzen](../7.3.Laravel_rutes_vistes.md#vistes) aquestes vistes en lloc de retornar text pla.

Instal·la amb [composer](../7.3.Laravel_rutes_vistes.md#enllaçant-amb-css-i-javascript-en-el-client) la llibreria **laravel/ui** , i utilitza-la per a incorporar **Bootstrap** al projecte.
Descàrrega Bootstrap amb npm install , i actualitza els arxius CSS i Javascript amb npm run dev
Incorpora els estils /css/app.css a la plantilla base del projecte, perquè els utilitzen totes les vistes que hereten d'ella.
Edita l'arxiu **partials/nav.blade.php** per a modificar la barra de navegació i deixar-la amb un estil particular de Bootstrap. Pots consultar aquesta [pàgina](https://getbootstrap.com/docs/4.5/components/navbar/) per a prendre idees d'alguns dissenys que pots aplicar en la barra de navegació.
Canvia de nom l'arxiu welcome.blade.php a inici.blade.php i canvia-ho perquè també herete de la plantilla base. Afig algun text introductori com a contingut. 

#### Exercisi 3 (Branca v1.3)

Sobre el projecte blog de la sessió anterior, afegirem aquests canvis:

* Crea un [controlador](../7.4.Laravel_controladors.md#controladors-de-recursos) de recursos (opció -r ) anomenat **PostController** , que ens servirà per a gestionar tota la lògica dels posts del blog.
* [Assigna automàticament](../7.4.Laravel_controladors.md#unint-totes-les-rutes-dun-controlador) amb el mètode **resource** cada ruta a la seua funció corresponent del controlador, en l'arxiu **routes/web.php** . Limita amb **only** les accions només a les funcions de llistat (index), fitxa (show), creació (create) i edició (edit).
* Utilitza el [proveïdor de serveis](../7.4.Laravel_controladors.md#reanomenant-les-rutes) **AppServiceProvider** per a "castellanitzar" les rutes de creació i edició, com en l'exemple que hem vist.
* Canvia de nom les [vistes](../7.4.Laravel_controladors.md#reanomenant-les-vistes) de **llistat** i **fitxa** d'un post a **index.blade.php** i show.blade.php, dins de la seua carpeta posts, i fes que els mètodes corresponents del controlador de posts [renderitzen](#renderitzant-les-vistes) aquestes vistes. 
* Per als mètodes create i edit , simplement retorna un text pla
indicant "Nou post" i "Edició de post", per exemple.
* Fes els canvis addicionals que siguen convenients (per exemple, en el menú de navegació) perquè els enllaços continuen funcionant, i prova que les quatre rutes (llistat, fitxa, creació i edició) funcionen adequadament.

#### Exercisi 4 (Branca v1.4)

Sobre el projecte blog anterior, afegirem aquests canvis:

* Fes que les funcions de **create** i **edit** del controlador de posts, en lloc de mostrar un missatge de text pla indicant que ací va un formulari, redirigisquen a la pàgina d'inici, usant la instrucció [**redirect**](../7.4.Laravel_controladors.md#utilitzar-la-resposta-per-fer-redireccions) .
* Afig un [helper](../7.4.Laravel_controladors.md#helperserviceprovider) al projecte que definisca una funció anomenada **fechaActual** . Rebrà com a paràmetre un format de data (per exemple, "d/m/i") i traurà la data actual en aquest format. Utilitza-ho per a mostrar la data actual en format "d/m/I" en la plantilla base, sota la barra de navegació, alineada a la dreta.

