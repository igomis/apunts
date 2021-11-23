# FRAMEWORK LARAVEL

* [Controladors](#controladors)
* [Injecció de dependències](#injecció-de-dependències)


## Controladors

Fins al moment hem vist solament com retornar una cadena per a una ruta i com associar una vista a una ruta directament en el fitxer de rutes. Però en general la forma recomanable de treballar serà associar aquestes rutes a un mètode d'un controlador. Els controladors permeten estructurar millor el codi de la nostra aplicació. La seua principal utilitat radica
a alliberar als arxius de rutes d'haver d'ocupar-se també de gestionar una certa lògica comuna de les
peticions, com l'accés a les dades, validació de formularis, etc. 

Com ja vam veure en la secció d'introducció, els controladors són el punt d'entrada de les peticions dels usuaris i són els que han de contenir tota la lògica associada al processament d'una petició, encarregant-se de realitzar les consultes necessàries a la base de dades, de preparar les dades i de cridar a la vista corresponent amb aquestes dades.

### Definició de controladors


Per a definir un controlador en la nostra aplicació, hem de tirar mà de nou del comando **php artisan** vist prèviament. En concret, utilitzarem l'opció **make:controller** seguida del nom que li vulguem donar al controlador. Típicament, els noms de controladors acaben amb el sufix Controller, per la qual cosa podem crear un de prova així:

```php
php artisan make:controller PruebaController
```

Això generarà una classe buida amb el nom del controlador. Per defecte, els controladors es guarden en la subcarpeta **app/Http/Controllers** del nostre projecte Laravel.


##### Nota
Per a que tot funcione correctament hem de descomentar la següent línea del RouteServiceProvider en providers

```php
protected $namespace = 'App\\Http\\Controllers';
```

#### Controladors d'un sol mètode (invoke)

El comando anterior admet alguns paràmetres addicionals més. Un molt útil és el paràmetre -i , que crea el controlador amb un mètode anomenat __invoke , que s'acte executa quan és anomenat des d'algun procés d'encaminament. Per exemple, si creem el controlador així:

```php
php artisan make:controller PruebaController -i
```

Es crearà la classe PruebaController en la carpeta app/Http/Controllers , amb un contingut com aquest:

```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class PruebaController extends Controller
{
...
public function __invoke(Request $request)
{
...
}
}

```

Dins del mètode __invoke podem definir la lògica de generar o obtindre les dades que necessita una vista, i renderitzar-la. Per exemple:

```php
public function __invoke(Request $request)
{
	$datos = array(...);
	return view('miVista', compact('datos'));
}
```

Així, en l'arxiu de rutes, n'hi ha prou amb definir la ruta que vulguem, i com segon paràmetre del mètode **get** , indicar el nom del controlador que es dispararà per a processar aqueixa ruta. Addicionalment,
també li podem assignar un nom a la ruta, com ja hem fet en exemples anteriors.

```php
Route::get('prueba', 'PruebaController')->name('prueba');
```

#### Controladors de multiples mètodes

##### Controladors de recursos


Si creem un controlador amb l'opció -r en lloc de l'opció -i utilitzada en l'exemple anterior, crearà un controlador de recursos ( resources ), i predefinirà en ell una sèrie de mètodes d'utilitat per a les operacions principals que es poden realitzar sobre una entitat de la nostra aplicació:

* index : mostra un llistat dels elements d'aqueixa entitat o recurs
* create : mostra el formulari per a donar d'alta nous elements
* store : emmagatzema en la base de dades el recurs creat amb el formulari anterior
* show : mostra les dades d'un recurs específic (a partir de la seua clau o id).
* edit : mostra el formulari per a editar un recurs existent
* update : actualitza en la base de dades el recurs editat amb el formulari anterior
* destroy : elimina un recurs pel seu identificador.

Òbviament, el codi de tots aquests mètodes apareixerà buit al principi, i els haurem d'emplenar amb les operacions corresponents més endavant.
Si volem utilitzar un controlador d'aquest tipus, i cridar a algun dels seus mètodes des d'alguna ruta, ja no n'hi ha prou amb posar el nom del controlador, com féiem abans amb els de tipus **invoke**, ja que ara hi ha més d'un mètode que triar. El que farem serà posar el nom del controlador, seguit d'una
arrova @ i el nom del mètode a invocar. Per exemple:

```php
Route::get('prueba', 'PruebaController@index')->name('listado_prueba');
```

#### Controladors API

Com a alternativa als controladors de recursos vistos abans, podem crear els controladors amb l'opció --api. Crearà un controlador amb els mateixos mètodes que el de recursos, excepte els mètodes create i edit , encarregats de mostrar els formularis de creació i edició de recursos, ja que en les APIs aquests formularis no són necessaris, com veurem en sessions posteriors.

#### Reanomenant les vistes

A mesura que el projecte creix, generarem un bon nombre de vistes associades a controladors, i és necessari estructurar aquestes vistes d'una forma adequada per a poder-les identificar ràpidament. Una convenció que podem seguir és nomenar les vistes a partir del controlador o model al qual fan referència, i a l'operació que realitzen. Per exemple, si tenim un controlador anomenat
PruebaController , se suposa que actuarà sobre una taula anomenada prueba (ho veurem més endavant, en la sessió d'accés a dades). I, podem emmagatzemar les vistes en la subcarpeta **resources/views/pruebas** , i definir dins les vistes
associades a cada operació del controlador que tinguem definida. Per exemple:
index.blade.php
show.blade.php
...

Paral·lelament, cada vegada que anem a carregar una vista des d'algun controlador o ruta, farem referència a aquest nom.

#### Unint totes les rutes dun controlador

Al final de tot el procés d'implementació d'un controlador (de recursos o de API) tindrem en l'arxiude rutes una dedicada a cada mètode del controlador (una per a index , una altra per a show , etc.).
Aquestes rutes poden agrupar-se en una sola usant el mètode resource de la classe Route , en lloc de get, indicant-li com a paràmetres el nom base de la ruta, i el controlador que s'encarregarà d'ella:

```
Route::resource('catalog', 'CatalogController');
```

La ruta anterior definirà una ruta GET cap a /catalog , atesa pel mètode index del controlador,una altra ruta GET cap a /catalog/{id} atesa pel mètode show del controlador... etc.
També podem utilitzar el mètode only per a indicar per a quins mètodes volem rutes:

```
Route::resource('catalog', 'CatalogController')->only(['index', 'show']);
```

Des del costat oposat, tenim disponible el mètode *except per a indicar que es generen totes les rutes excepte aquelles per als mètodes indicats:

```
Route::resource('catalog', 'catalogController')->except(['update', 'edit']);
```

Amb els controladors de tipus API també podem generar automàticament totes les rutes per als seus mètodes, utilitzant el mètode apiResource de la classe Route , en lloc del mètode resource empleat abans:

```
Route::apiResource('prueba', 'PruebaController');
```

#### Controladors i espais de noms

També podem crear sub-carpetes dins de la carpeta Controllers per a organitzar-nos millor. En aquest cas, l'estructura de carpetes que creem no tindrà res a veure amb la ruta associada a la petició i, de fet, a l'hora de fer referència al controlador únicament haurem de fer-ho a través del seu espai de noms.

Com hem vist en referenciar el controlador en el fitxer de rutes únicament hem d'indicar el seu nom i no tota la ruta ni l'espai de noms App\Http\Controllers. Açò és perquè el servei encarregat de carregar les rutes afig automàticament l'espai de noms arrel per als controladors. 
Si fiquem tots els nostres controladors dins del mateix espai de noms no haurem d'afegir res més. Però si decidim crear sub-carpetes i organitzar els nostres controladors en sub-espais de noms, llavors sí que haurem d'afegir aqueixa part.

Per exemple, si vam crear un controlador en **App\Http\Controllers\Photos\AdminController**, llavors per a registrar una ruta fins a aquest controlador hauríem de fer:

```
	Route::get('foo', 'Photos\Admincontroller@method');
```

## Injecció de dependències

El concepte d'injecció de dependències és molt habitual en l'ús de frameworks. Consisteix en un mecanisme que facilita recursos als diferents components de l'aplicació, i és una cosa que ja hem utilitzat, sense saber-ho, en els mètodes que s'han generat per als controladors.
Per exemple, quan definim un mètode en un controlador que necessita processar una petició, se li passa
com a paràmetre un objecte de tipus **Request**. Automàticament, Laravel processa el tipus de dada i
obté l'objecte associat (en aquest cas, la petició del client).

```
class PruebaController extends Controller
{
	...
	public function store(Request $request)
	{
		...
	}
}
```

### Exemple: La resposta

Igual que tenim un objecte Request per a obtindre dades de la petició, també existeix un **Response** per a gestionar la resposta. Laravel proporciona un mètode **response** al qual li podem passar diversos paràmetres:

1. El contingut de la resposta
2. El codi d'estat HTTP de resposta (si no s'especifica, per defecte és 200)
3. Un array amb les capçaleres de resposta (per defecte està buit).

Així, si per exemple volem emetre una resposta determinada amb el seu codi d'estat des d'un
controlador, podem fer això (per exemple, per a un codi 201):

```
response("Mensaje de respuesta", 201);
```
Les capçaleres poden especificar-se com un array, o enllaçant anomenades al mètode **header** (una per a cada capçalera):

```
response("Mensaje de respuesta", 201)
->header('Cabecera1', 'Valor1')
->header('Cabecera2', 'Valor2');
```
En el cas de voler retornar un objecte com a resposta, podem emprar el mètode **json** de la resposta (més endavant veurem que tots els objectes emesos directament al client s'envien en
format **JSON**), i així podrem adjuntar un codi d'estat diferent de 200:

```
return response()->json(['datos' => datos], 201)
->header('Cabecera1', 'Valor1')
...;
```

### Utilitzar la resposta per fer redireccions


Existeix també un mètode redirect que podem emprar per a redirigir a una ruta des d'una altra,bé especificant la ruta com a paràmetre...
```
redirect('/');
```

... o bé indicant una ruta amb nom:

```
redirect()->route('inicio');
```

... O si volem tornar a la ruta anterior simplement podem usar el mètode back: 

```	
	return back();
```


Podem passar valors a la següent redirecció, emmagatzemant-los en sessió amb el mètode **with** ,
encara que aquests valors es perdran en la següent petició (no es queden emmagatzemats en sessió):

```
redirect()->route('inicio')
->with('mensaje', 'Mensaje enviado correctamente');
```


Per a accedir a aquest missatge des de la vista afectada, hem d'utilitzar la funció session :

```
@if(session()->has('mensaje'))
	{{ "{{  session('mensaje') " }}}}
@endif
```


Finalment, notar que si fem la redirecció des de dins d'un mètode d'un controlador (per exemple, per a redigir a una ruta des d'una altra), haurem de retornar ( return ) el resultat d'aqueixa
redirecció perquè faça efecte:

```
class LibroController extends Controller
{
	public function index()
	{
		...
	}
	public function store(...)
	{
		...
		return redirect()->route('libros.index');
	}
}
```


#### Redirecció amb els valors de la petició
Les redireccions se solen utilitzar després d'obtenir algun error en la validació d'un formulari o després de processar alguns paràmetres d'entrada. En aquest cas, perquè en mostrar el formulari amb els errors produïts puguem afegir les dades que havia escrit l'usuari haurem de tornar a enviar els valors enviats amb la petició usant el mètode withInput():

```
	return redirect('form')->withInput();
	// O per a reexpedir les dades d'entrada excepte alguns:
	return redirect('form')->withInput($request->except('password'));
```
	

### Exemple:Helpers

Per a acabar aquesta introducció al que suposa la injecció de dependències en frameworks de desenvolupament, farem ús d'una eina que ens pot ser útil en algunes situacions: els **helpers**.
Un helper és bàsicament una funció d'utilitat que podem voler utilitzar en diversos punts de la
nostra web, i que necessitem tindre localitzada i compartida. Per exemple, imaginem que volem
ressaltar en el nostre menú de navegació l'opció que tenim actualment visible.
Per a això, podem definir una classe **CSS** amb l'estil que vulguem per a ressaltar (això ho farem a part, en els arxius **CSS** del projecte), i després utilitzar aqueixa classe **CSS** en una condició per a cada menú de navegació.

Per exemple, suposem que la classe **CSS** per a identificar el menú actiu es diu actiu . En aquest cas,
per a un menú de diverses opcions com aquest, n'hi ha prou amb utilitzar el mètode **routeIs** de la petició
( **request** ) per a comprovar si la ruta coincideix amb cada menú, i mostrar-lo com a actiu o no, usant un
operador ternari de comparació:

```html
<nav>
	<ul>
		<li class="{{ "{{  request()->routeIs('inicio') ? 'activo' : '' " }}}}">
			<a href="/">Inicio</a>
		</li>
		<li class="{{ "{{  request()->routeIs('contacto') ? 'activo' : '' " }}}}">
			<a href="/contacto">Contacto</a>
		</li>
		...
	</ul>
</nav>
```
Aquesta característica també funciona si les rutes tenen paràmetres.
Podem, en canvi, traure fora de la vista la lògica d'establir un camp com a actiu o no. Per a això,
creem un arxiu d'utilitat o **helper**. Ho podem cridar **helpers.php** , i situar-ho en la mateixa
carpeta app, dins d'una carpeta **Helpers**. Dins, definim la funció que ens retornarà si una ruta està activa o no, a partir del seu nom:

```php
function setActivo($nombreRuta)
{
	return request()->routeIs($nombreRuta) ? 'activo' : '';
}
```

I d'aquesta manera, la nostra vista simplement es dedica a cridar a aquesta funció per a cada element del menú:

```html
<nav>
	<ul>
		<li class="{{ "{{  setActivo('inicio') " }}}}">
			<a href="/">Inicio</a>
		</li>
		<li class="{{ "{{  setActivo('contacto') " }}}}">
			<a href="/contacto">Contacto</a>
		</li>
		...
	</ul>
</nav>
```

En el cas de voler mantindre l'enllaç actiu per a qualsevol subruta a partir de l'original (per exemple,
quan estem veient la fitxa d'un registre a partir del llistat general, podem utilitzar el wildcard d'asterisc
* ):

```html
<li class="{{ "{{  setActivo('peliculas.*') " }}}}">
	<a href="{{ "{{  route('peliculas') " }}}}">Peliculas</a>
</li>
```

No obstant això, perquè Laravel carregue l'arxiu **helpers.php** que acabem de crear, com no és una classe, hem d'indicar-ho explícitament (Laravel càrrega automàticament totes les classes de la carpeta app , però no arxius solts que no siguen classes). Podem fer-ho amb l'autoloader del composer.json de l'arrel del nostre projecte. En la secció autoload cal afegir una secció **files** amb un **array** amb els
arxius que vulguem que es carreguen també:

```
	"autoload": {
	"classmap": [ ... ],
	"psr-4": { ... },
	"files": ["app/helpers.php"]
	},
```

Després d'efectuar el canvi, hem de dir-li a *composer que torne a compilar l'acte carregador. Des de la
carpeta del projecte, executem aquest comando:

```
composer dump-autoload
```

##### HelperServiceProvider

Però la millor forma de fer-ho per a que les funcions d'aquest fitxer siguen visibles en l'aplicació és a crear un ServiceProvider i a registrar-lo:

	php artisan make:provider HelperServiceProvider
	
Crearà un fitxer en la carpeta Providers.
L'editem per afegir el nostre Helper.

```php

	/**
	     * Register services.
	     *
	     * @return void
	     */
	    public function register()
	    {
	        require_once base_path() . '/app/Helpers/myHelpers.php';
	    }
```

	    
I ara registrem el ServiceProvider en el fitxer **config/app.php**

```php

'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        
        .....

        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\AuthServiceProvider::class,
        // App\Providers\BroadcastServiceProvider::class,
        App\Providers\EventServiceProvider::class,
        App\Providers\RouteServiceProvider::class,
        App\Providers\HelperServiceProvider::class,

    ],
```
    
I com cada volta que toquen un fitxer de configuració executem

	php artisan config:cache
	







