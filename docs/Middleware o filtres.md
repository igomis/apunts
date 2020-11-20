### Middleware o filtres

Els components anomenats Middleware són un mecanisme proporcionat per Laravel per a filtrar les peticions HTTP que es realitzen a una aplicació. 
Un filtre o middleware es defineix com una classe PHP emmagatzemada en un fitxer dins de la carpeta **app/Http/Middleware**. 
Cada middleware s'encarregarà d'aplicar un tipus concret de filtre i de decidir que realitzar amb la petició realitzada: 

* permetre la seua execució
* donar un error
* redireccionar a una altra pàgina en cas de no permetre-la.

Laravel inclou diversos filtres per defecte, un d'ells és l'encarregat de realitzar **l'autenticació dels usuaris**. Aquest filtre ho podem aplicar sobre **una ruta**, un conjunt de **rutes** o sobre un **controlador** en concret. 

Aquest middleware s'encarregarà de filtrar les peticions a aquestes rutes: en cas d'estar loguegat i tenir permisos d'accés li permetrà continuar amb la petició, i en cas de no estar autenticat el redireccionará al formulari de login.

Laravel inclou middleware per a **gestionar l'autenticació**, el **mode manteniment**, la **protecció contra CSRF**, i alguns mes. Tots aquests filtres els podem trobar en la carpeta **app/Http/Middleware**, els quals els podem modificar o ampliar la seua funcionalitat. Però a més d'aquests podem crear els nostres propis Middleware com veurem a continuació.

#### Definir un nou Middleware
Per a crear un nou Middleware podem utilitzar el comando de Artisan: 

	php artisan make:middleware MyMiddleware

Aquest comanament crearà la classe MyMiddleware dins de la carpeta app/Http/Middleware amb el següent contingut per defecte:

	<?php

	namespace App\Http\Middleware;
	use Closure;

	class MyMiddleware
	{
    	/**
     	* Handle an incoming request.
     	*
     	* @param  \Illuminate\Http\Request  $request
     	* @param  \Closure  $next
     	* @return mixed
     	*/
    	public function handle($request, Closure $next)
    	{
        	return $next($request);
    	}	
	}
	
El codi generat per Artisan ja ve preparat perquè puguem escriure directament la implementació del filtre a realitzar dins de la funció handle. Com podem veure, aquesta funció solament inclou el valor de tornada amb una trucada a return **$next($request);** , que el que fa és continuar amb la petició i executar el mètode que ha de processar-la. Com a entrada la funció handle rep dos paràmetres:

* $request: En la qual ens vénen tots els paràmetres d'entrada de la peticion.
* $next: El mètode o funció que ha de processar la petició.

Per exemple podríem crear un filtre que redirigisca al home si l'usuari té menys de 18 anys i en un altre cas que li permeta accedir a la ruta:

	public function handle($request, Closure $next) {
		if ($request->input('age') < 18) { 
			return redirect('home');
		}
		return $next($request); 
	}
	
Com hem dit abans, podem fer tres coses amb una petició:

* Si tot és correcte permetre que la petició continue retornant: return $next($request);
* Realitzar una redirecció a una altra ruta per a no permetre l'accés amb: return redirect('home');
* Llançar una excepció o cridar al mètode abort per a mostrar una pàgina d'error: abort(403, 'Unauthorized
action.');


#### Ús de Middleware
De moment hem vist perquè val i com es defineix un Middleware, en aquesta secció veurem com utilitzar-los. Laravel permet la utilització de Middleware de tres formes diferents: global, associat a rutes o grups de rutes, o associat a un controlador o a un mètode d'un controlador. 
En els tres casos serà necessari registrar primer el Middleware en la classe **app/Http/Kernel.php**.

##### Middleware global
Per a fer que un Middleware s'execute amb totes les peticions HTTP realitzades a una aplicació simplement ho hem de registrar en el array **$middleware** definit en la classe app/Http/Kernel.php. 

Per exemple:

	protected $middleware = 
	[ \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class, 
		\App\Http\Middleware\EncryptCookies::class, 
		\Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class, 
		\Illuminate\Session\Middleware\StartSession::class, 
		\Illuminate\View\Middleware\ShareErrorsFromSession::class,
		\App\Http\Middleware\VerifyCsrfToken::class, 
		\App\Http\Middleware\MyMiddleware::class,
	];

En aquest exemple hem registrat la classe MyMiddleware al final del array. Si volem que el nostre middleware s'execute abans que un altre filtre simplement haurem de col·locar-ho abans en la posició del array.

#### Middleware associat a rutes
En el cas de voler que el nostre middleware s'execute solament quan es cride a una ruta o a un grup de rutes també haurem de registrar-ho en el fitxer **app/Http/Kernel.php**, però en el array **$routeMiddleware**. En afegir-ho a aquest array a més haurem d'assignar-li un nom o clau, que serà el que després utilitzarem associar-ho amb una ruta.

En primer lloc afegim el nostre filtre al array i li assignem el nom "CheckAge":

	protected $routeMiddleware = [
	'auth' => \App\Http\Middleware\Authenticate::class,
	'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class, 
	'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
	'CheckAge' => \App\Http\Middleware\MyMiddleware::class,
	];

Una vegada registrat el nostre middleware ja ho podem utilitzar en el fitxer de rutes **app/Http/routes.php** mitjançant la clau o nom assignat, per exemple:

	Route::get('profile', [
	'middleware' => 'checkAge',
	'uses' => 'Usercontroller@showprofile'
	]);
	
Si volem associar diversos middleware amb una ruta simplement hem d'afegir un array amb les claus. Els filtres s'executaran en l'ordre indicat en aquest array:

	Route::get('profile', [
	'middleware' => ['Auth','checkAge'],
	'uses' => 'Usercontroller@showprofile'
	]);
	
Laravel també permet associar els filtres amb les rutes usant el mètode middleware() sobre la definició de la ruta de la forma:

	Route::get('/', function () { // ...})->middleware(['first', 'second']);
	
	// O sobre un controlador:
	
	Route::get('profile', 'Usercontroller@showprofile')->middleware('auth');
	
##### Middleware dins de controladors

També és possible indicar el middleware a utilitzar des de dins d'un controlador. En aquest cas els filtres també hauran d'estar registrador en el array **$routeMiddleware** del fitxer app/Http/Kernel.php. 

Per a utilitzar-los es recomana realitzar l'assignació en el constructor del controlador i assignar els filtres usant la seua clau mitjançant el mètode middleware. Podrem indicar que es filtren tots els mètodes, solament alguns, o tots excepte els indicats, per exemple:

	class UserController extends Controller {
	/
 	Instantiate a new UserController instance. 
 	@return void
	/
	public function __construct() {
		// Filtrar tots els mètodes 
		$this->middleware('auth');
		// Filtrar solament aquests mètodes...
		$this->middleware('log', ['only' => ['fooAction', 'barAction']]);
		// Filtrar tots els mètodes excepte...
		$this->middleware('subscribed', ['except' => ['fooAction', 'barAction']]); }
	}

#### Revisar els filtres assignats
En crear una aplicació Web és important assegurar-se que totes les rutes definides són correctes i que les parts privades realment estan protegides. Per a açò Laravel inclou el següent mètode de Artisan:

	php artisan route:list

Aquest mètode mostra una taula amb totes les rutes, mètodes i accions. A més per a cada ruta indica els filtres associats, tant si estan definits des del fitxer de rutes com des de dins d'un controlador. Per tant és molt útil per a comprovar que totes les rutes i filtres que hem definit s'hagen creat correctament.

#### Pas de paràmetres
Un Middleware també pot rebre paràmetres. Per exemple, podem crear un filtre per a comprovar si l'usuari loguejat té un determinat rol indicat per paràmetre. Per a açò el primer que hem de fer és afegir un tercer paràmetre a la funció handle del Middleware:

	<?php

	namespace App\Http\Middleware;
	use Closure;

		class CheckRole
		{
    	/**
     	* Handle the incoming request.
     	*
     	* @param  \Illuminate\Http\Request  $request
     	* @param  \Closure  $next
     	* @param  string  $role
     	* @return mixed
     	*/
    		public function handle($request, Closure $next, $role)
    		{
        		if (! $request->user()->hasRole($role)) {
            	// Redirect...
       		}
				return $next($request);
    		}

		}
En el codi anterior d'exemple s'ha afegit el tercer paràmetre $role a la funció. Si el nostre filtre necessita rebre més paràmetres simplement hauríem d'afegir-los de la mateixa forma a aquesta funció.

Per a passar un paràmetre a un middleware en la definició d'una ruta ho haurem d'afegir a continuació del nom del filtre separat per dos punts, per exemple:

	Route::put('post/{id}', ['middleware' => 	'role:editor', function ($id) { //
	}]);
	
Si hem de passar més d'un paràmetre al filtre els separarem per comes, per exemple: role:editor,admin.

### Rutes avançades

Laravel permet crear grups de rutes per a especificar opcions comunes a totes elles, com per exemple un middleware, un prefix, un subdomini o un espai de noms que s'ha d'aplicar sobre totes elles.

A continuació anem a veure algunes d'aquestes opcions, en tots els casos usarem el mètode Route::group, el qual rebrà com a primer paràmetre les opcions a aplicar sobretot el grup i com a segon paràmetre una clausula amb la definició de les rutes.

#### Middleware sobre un grup de rutes

Aquesta opció és molt útil per a aplicar un filtre sobretot un conjunt de rutes, d'aquesta forma solament haurem d'especificar el filtre una vegada i a més ens permetrà dividir les rutes en seccions (distingint millor al fet que seccions se'ls està aplicant un filtre):

	Route::group(['middleware' => 'auth'], function () 		{ 
		Route::get('/', function () {
		// Ruta filtrada pel middleware });
		Route::get('user/profile', function () { // Ruta 		filtrada pel middleware
		}); 
	});
	
#### Grups de rutes amb prefix

També podem utilitzar l'opció d'agrupar rutes per a indicar un prefix que s'afegirà a totes les URL del grup. Per exemple, si volem definir una secció de rutes que comencen pel prefix dashboard hauríem de fer el següent:

	Route::group(['prefix' => 'dashboard'], function () 		{ Route::get('catalog', function () 		{ / ... / }); Route::get('users', function () 		{ / ... / });
	});
	
També podem crear grups de rutes dins d'altres grups. Per exemple per a definir un grup de rutes a utilitzar en una API i crear diferents rutes segons la versió de la API podríem fer:

	Route::group(['prefix' => 'api'], function() {
		Route::group(['prefix' => 'v1'], function() {
		// Rutes amb el prefix api/v1
			Route::get('recurs', 'ControllerAPIv1@getRecurso'); 
			Route::post('recurs', 'ControllerAPIv1@postRecurso'); 
			Route::get('recurs/{id}', 'ControllerAPIv1@putRecurso');
		});
		Route::group(['prefix' => 'v2'], function() {
		// Rutes amb el prefix api/v2
			Route::get('recurs', 'ControllerAPIv2@getRecurso'); 
			Route::post('recurs', 'ControllerAPIv2@postRecurso'); 
			Route::get('recurs/{id}', 'ControllerAPIv2@putRecurso');
		}); 
	});
D'aquesta forma podem crear seccions dins del nostre fitxer de rutes per a agrupar, per exemple, totes les rutes públiques, totes les de la secció privada d'administració, secció privada d'usuari, les rutes de les diferents versions de la API del nostre lloc, etc.
