# FRAMEWORK LARAVEL

## [Base de dades](https://laravel.com/docs/8.x/database)

Laravel facilita la configuració i l'ús de diferents tipus de base de dades: MySQL, Postgres, SQLite i SQL Server. En el fitxer de configuració (**config/database.php**) hem d'indicar tots els paràmetres d'accés a les nostres bases de dades i a més especificar com és la connexió que s'utilitzarà per defecte. En Laravel podem fer ús de diverses bases de dades alhora, encara que siguen de diferent tipus. Per defecte s'accedirà a la qual especifiquem en la configuració i si volem accedir a una altra connexió ho haurem d'indicar expressament en realitzar la consulta.

En aquest capítol veurem com configurar una base de dades, com crear taules i especificar els seus camps des de codi, com inicialitzar la base de dades i com construir consultes tant de forma directa com a través del ORM anomenat **Eloquent**.

### Configuració inicial

##### Configuració de la Base de dades
El primer que hem de fer per a treballar amb bases de dades és completar la configuració. Com a exemple anem a configurar l'accés a una base de dades tipus **MySQL**. Si editem el fitxer amb la configuració config/database.php podem veure en primer lloc la següent línia:

	'default' => env('DB_CONNECTION', 'mysql'),

Aquest valor indica el tipus de base de dades a utilitzar per defecte. Com vam veure en el primer capítol Laravel utilitza el sistema de variables d'entorn per a separar les diferents configuracions d'usuari o de màquina. 
El mètode env('DB_CONNECTION', 'mysql') el que fa és obtenir el valor de la variable DB_CONNECTION del fitxer **.env**. En cas que aquesta variable **no estiga definida** retornarà el valor per defecte mysql.

En aquest mateix fitxer de configuració, dins de la secció connections, podem trobar tots els camps utilitzats per a configurar cada tipus de base de dades, en concret la base de dades tipus mysql té els següents valors:

```php
	'mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
        ],
```        

##### Contrasenya d'accés
Com es pot veure, bàsicament els camps que hem de configurar per a usar la nostra base de dades són: host, database, username i password. El host ho podem deixar com està si anem a usar una base de dades local, mentre que els altres tres camps sí que hem d'actualitzar-los amb el noms de la base de dades a utilitzar i l'usuari i la contrasenya d'accés. Per a posar aquests valors obrim el fitxer **.env** de l'arrel del projecte i els actualitzem:

	DB_CONNECTION=mysql 
	DB_HOST=localhost 
	DB_DATABASE=nom-base-de-dades 
	DB_USERNAME=nom-de-usuari
	DB_PASSWORD=contrasenya-de-accés
	
##### Crear la base de dades
L'únic pas necessari des de fora de Laravel per a accedir a la base de dades serà crear-la. La resta d'operacions(creació de taules, camps, claus, relacions, dades, etc) es podran fer des del propi
Laravel, com anirem veient a continuació.
La base de dades podem crear-la a través d'algun administrador que tinguem disponible (per exemple,
**phpMyAdmin** per a bases de dades **MySQL**), o bé per línia de comandos, connectant amb el SGBD en
qüestió i creant la base de dades.


## Migracions

Les migracions són un sistema de control de versions per a bases de dades. Permeten que un equip treballe sobre una base de dades afegint i modificant camps, mantenint un històric dels canvis realitzats i de l'estat actual de la base de dades. 

Les migracions s'utilitzen de forma conjunta amb l'eina Schema builder (que veurem en la següent secció) per a gestionar l'esquema de base de dades de l'aplicació.

La forma de funcionar de les migracions és crear fitxers (PHP) amb la descripció de la taula a crear i posteriorment, si es vol modificar aquesta taula s'afegiria una nova migració (un nou fitxer PHP) amb els camps a modificar.

Artisan inclou comandos per a crear migracions, per a executar les migracions o per a fer rollback de les mateixes (tornar arrere).

#### Estructura de les migracions

Per defecte, Laravel porta unes migracions predefinides, que es troben en la carpeta
**database/migrations** . Cadascuna té un nom d'arxiu que comença per la data en què es va fer, seguida d'una breu descripció del que conté (creació de la taula d'usuaris, reinicialitze de contrasenyes...).
Pot ser que algunes d'aquestes migracions no ens vagen a ser necessàries, amb el que podem esborrar-les directament, i pot ser que altres (especialment la creació de la taula d'usuaris) sí que ens servisca, però amb altres camps, amb el que haurem d'editar-la, com veurem a continuació.
Si examinem el contingut d'una migració, totes han de tindre dos mètodes:

* **up** : permet agregar taules, columnes o índexs a la base de dades
* **down** : reverteix el fet pel mètode anterior

Si observem el contingut d'un mètode up dels quals vénen predefinits per a crear una taula, veiem
que s'utilitzen diferents mètodes per a definir els tipus de dades de cada camp de la taula, com per exemple **id()** per a camps que puguen contindre sencers *autoincrementales, o **string()** per a camps
de tipus text. A més, existeixen altres mètodes modificadors per a agregar propietats addicionals,
com per exemple **unique()** per a indicar valors únics (claus alternatives), o **nullable()** per a indicar
que un camp admet nuls. Ací tenim un exemple de mètode **up** :

```php
public function up(){	Schema::create('usuarios', function(BluePrint $tabla) {		$tabla->id();		$tabla->string('nombre');		$tabla->string('email')->unique();		...		$tabla->timestamps();	});}
```
Per defecte, com veiem en els exemples que es proporcionen, els esquemes es creen amb un id
autonumèric, i uns **timestamps** per a indicar la data de creació i de modificació de cada registre, i que
Laravel gestiona de manera automàtica quan inserim o actualitzem continguts, la qual cosa resulta molt
útil.

Sobre aquesta base, podem afegir o llevar els camps que vulguem. Per a veure els tipus disponibles per a les
columnes de la taula, podem visitar la documentació de Laravel sobre migracions, en concret
buscarem el subapartat **Available Column Types**. 

Convé tindre present, per exemple, que el tipus **string** que hem utilitzat en l'exemple anterior té una limitació de 255 caràcters. Per a textos
més grans, es pot emprar el tipus **text** (20.000 caràcters aproximadament) o **longText** .
Podem especificar una clau primària amb el mètode **primary** , al qual li podem passar o bé el
nom del camp clau, o un array de camps clau, en el cas que aquesta siga composta. 
Per defecte, els
camps de tipus **id** s'acte-estableixen com a claus primàries.

```
$table->primary(['campo1', 'campo2']);
```

#### Crear una nova migració

Per a crear una nova migració s'utilitza el comando de Artisan **make:migration**, al com li passarem el nom del fitxer a crear i el nom de la taula:

```
	php artisan make:migration nom_migracio
```	
	
Açò ens crearà un fitxer de migració en la carpeta database/migrations amb el nom **<TIMESTAMP>_nom_migracio.php**. En afegir un timestamp a les migracions el sistema sap l'ordre en el qual ha d'executar (o desfer) les mateixes.

Notar que Laravel ja assigna automàticament la data de la migració, només hem d'especificar el nom
descriptiu d'aquesta. A més, si Laravel detecta la paraula create en el nom de la migració,
finalitzada en table, intueix que és per a crear una taula nova. 
En canvi, si detecta la paraula to (entre
altres), i al final la paraula table, intueix que s'alterarà o modificar una taula existent. Això és gràcies a la classe **TableGuesser** incorporada en Laravel, que detecta uns certs patrons en els noms de migracions.

La diferència entre la creació i la modificació és que en el mètode **up** de la migració s'utilitzarà
**Schema::create** o **Schema::table** sobre la taula en qüestió, respectivament.

En qualsevol cas, també podem especificar un paràmetre addicional en el comando de migració per a indicar
si volem crear o modificar una taula, i d'aquesta manera podem definir el nom de la migració
en l'idioma que vulguem, i sense restriccions de patrons. Aquestes dues migracions creen una taula (comandes) i modifiquen una altra (usuaris), respectivament:

```
php artisan make:migration crear_tabla_pedidos --create=pedidosphp artisan make:migration nuevo_campo_usuario --table=usuarios	
```
En el cas de la segona migració, si, per exemple, volem afegir una columna amb el número de telèfon
 dels usuaris, pot quedar així (tant el mètode up com el down ):
 
``` 
public function up(){	Schema::table('usuarios', function(Blueprint $tabla) 	{		$tabla->string('telefono')->nullable();	});}public function down(){	Schema::table('usuarios', function(Blueprint $tabla) 	{		$tabla->dropColumn('telefono');	});}
```


Si volem que el camp en qüestió estiga en un ordre concret, podem usar el mètode after per a indicar
darrere de quin camp volem posar-ho (en el mètode up ):

```
$tabla->string('telefono')->after('email')->nullable();
```
 
#### Executar i esborrat de migracions

Després de crear una migració i de definir els camps de la taula (en la següent secció veurem com especificar açò) hem de llançar la migració amb el següent comando:

	php artisan migrate

Aquest comanament aplicarà la migració sobre la base de dades. Si hi haguera més d'una migració pendent s'executaran totes. Per a cada migració es dirà al seu mètode up perquè creu o modifique la base de dades. Posteriorment en cas que vulguem desfer els últims canvis podrem executar:

	php artisan migrate:rollback
	
	

O si volem desfer totes les migracions 

	php artisan migrate:reset

Un comanament interessant quan estem desenvolupant un nou lloc web és **migrate:refresh**, el qual desfarà tots els canvis i tornar a aplicar les migracions:

	php artisan migrate:fresh
	
A més si volem comprovar l'estat de les migracions, per a veure les que ja estan instal·lades i les que queden pendents, podem executar:

	php artisan migrate:status

### Migració inversa

Si tenim la base de dades i volem crear les migracions a partir d'ella podem instal·lar el següent [component](https://github.com/oscarafdev/migrations-generator). Te un bug ,explicat en aquest [enllaç](https://github.com/Xethron/migrations-generator/issues/191).



### Schema Builder

Una vegada creada una migració hem de completar els seus mètodes up i down per a indicar la taula que volem crear o el camp que volem modificar. En el mètode down sempre haurem d'afegir l'operació inversa, eliminar la taula que s'ha creat en el mètode up o eliminar la columna que s'ha afegit. 

Açò ens permetrà desfer migracions deixant la base de dades en el mateix estat en el qual es trobaven abans que s'afegiren.

Per a especificar la taula a crear o modificar, així com les columnes i tipus de dades de les mateixes, s'utilitza la classe **Schema**. Aquesta classe té una sèrie de mètodes que ens permetrà especificar l'estructura de les taules independentment del sistema de base de dades que utilitzem.

##### Crear i esborrar una taula
Per a afegir una nova taula a la base de dades s'utilitza el següent constructor:

	Schema::create('users', function (Blueprint $table) 	{ 
		$table->increments('id');
	});
	
On el primer argument és el nom de la taula i el segon és una funció que rep com a paràmetre un objecte
del tipus Blueprint que utilitzarem per a configurar les columnes de la taula.

En la secció down de la migració haurem d'eliminar la taula que hem creat, per a açò usarem algun dels
següents mètodes: 

	Schema::drop('users');
	Schema::dropIfExists('users');
		
En crear una migració amb el comando de Artisan make:migration ja ens ve aquest codi afegit per defecte, la creació i eliminació de la taula que s'ha indicat i a més s'afigen un parell de columnes per defecte (id i timestamps).

##### Afegir columnes
El constructor Schema::create rep com a segon paràmetre una funció que ens permet especificar les columnes que va a tenir aquesta taula. 

En aquesta funció podem anar afegint tots els camps que vulguem, indicant per a cadascun d'ells el seu tipus i nom, i a més si volem també podrem indicar una sèrie de modificadors com a valor per defecte, índexs, etc. Per exemple:

```php
	Schema::create('users', function($table) {
		$table->increments('id'); 
		$table->string('username', 32); 
		$table->string('password'); 
		$table->smallInteger('vots'); 
		$table->string('direccion'); 
		$table->boolean('confirmat')->default(false);
		$table->timestamps();
	});
```	
	
Schema defineix molts tipus de dades que podem utilitzar per a definir les columnes d'una taula, alguns dels principals són:

|Comando|Tipus de camp|
|--|--|
|$table->boolean('confirmed');| BOOLEAN|
|$table->enum('choices', array('foo', 'bar'));| ENUM |
|$table->float('amount');| FLOAT|
|$table->increments('id');| Clau principal tipus INTEGER amb Acte-Increment| 
|$table->integer('votes');| INTEGER|
|$table->mediumInteger('numbers');| MEDIUMINT|
|$table->smallInteger('votes');| SMALLINT|
|$table->tinyInteger('numbers');| TINYINT|
|$table->string('email');| VARCHAR|
|$table->string('name', 100);| VARCHAR amb la longitud indicada TEXT|
|$table->text('description');| TEXT|
|$table->timestamp('added_on');| |TIMESTAMP|
|$table->timestamps();|Afig els timestamps "created_at" i "updated_at"|
|->nullable()|Indicar que la columna permet valors NULL|
|->default($value)|Declara un valor per defecte per a una columna|
|->unsigned()|Afig UNSIGNED a les columnes tipus INTEGER|

Els tres últims es poden combinar amb la resta de tipus per a crear, per exemple, una columna que permeta nuls, amb un valor per defecte i de tipus unsigned.

Per a consultar tots els tipus de dades que podem utilitzar podeu consultar la documentació de Laravel [en](http://laravel.com/docs/8.x/migrations#creating-columns):

#### Afegir índexs
Schema suporta els següents tipus d'índexs:

| Comanament| Descripció|
|--|--|
|$table->primary('id');| Afegir una clau primària|
|$table->primary(array('first', 'last'));| Definir una clau primària composta|
|$table->unique('email');|  Definir el camp com UNIQUE|
|$table->index('state');|Afegir un índex a una columna|

En la taula s'especifica com afegir aquests índexs després de crear el camp, però també permet indicar aquests índexs alhora que es crea el camp:

	$table->string('email')->unique();

##### Claus alienes
Amb Schema també podem definir claus alienes entre taules:
 
	$table->integer('user_id')->unsigned();
	$table->foreign('user_id')->references('id')->on('users');
	
En aquest exemple en primer lloc afegim la columna "user_id" de tipus UNSIGNED INTEGER (sempre haurem de crear primer la columna sobre la qual es va a aplicar la clau aliena). A continuació vam crear la clau aliena entre la columna "user_id" i la columna "id" de la taula "users".

La columna amb la clau aliena ha de ser del **mateix tipus** que la columna a la qual apunta. Si per exemple vam crear una columna a un índex auto-incremental haurem d'especificar que la columna siga **unsigned** per que no es produïsquen errors.

També podem especificar les accions que s'han de realitzar per a "**on delete**" i "**on update**":

	$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');

Per a eliminar una clau aliena, en el mètode down de la migració hem d'utilitzar el següent codi: 

	$table->dropForeign('posts_user_id_foreign');

Per a indicar la clau aliena a eliminar hem de seguir el següent patró per a especificar el nom **<tabla>_<columna>_foreign**. On "taula" és el nom de la taula actual i "columna" el nom de la columna sobre la qual es cree la clau aliena.




## [Models de dades mitjançant ORM](https://laravel.com/docs/8.x/eloquent)

El mapeado objecte-relacional (més conegut pel seu nom en anglès, Object-Relational mapping, o per les seues sigles ORM) és una tècnica de programació per a convertir dades entre un llenguatge de programació orientat a objectes i una base de dades relacional com a motor de persistència. Açò possibilita l'ús de les característiques pròpies de l'orientació a objectes, podrem accedir directament als camps d'un objecte per a llegir les dades d'una base de dades o per a inserir-los o modificar-los.
Laravel inclou el seu propi sistema de ORM anomenat **Eloquent**, el qual ens proporciona una manera elegant i fàcil d'interactuar amb la base de dades. Per a cada taula de la base dades haurem de definir el seu corresponent **model**, el qual s'utilitzarà per a interactuar des de codi amb la taula.

### Definició dun model de dades

Per defecte els models es guardaran com a classes PHP dins de la carpeta **app/Models**.

```
	php artisan make:model Movie
```

Aquest comando crearà el fitxer movie.php dins de la carpeta **app/Models** amb el codi bàsic d'un model.

```php
<?php	namespace App\Models;	use Illuminate\Database\Eloquent\Model;	class Movie extends Model	{
	...	}?>
```

Automàticament, s'associa aquest model a una taula amb el mateix nom, però en plural i en minúscula,
per la qual cosa els models anteriors estarien associats a unes taules llibres i usuaris en la base de dades, respectivament. En cas que no vulguem que siga així, definim una propietat \$table en la classe amb el nom que vulguem que tinga la taula associada. Per exemple:

```php
class Movie extends Model{	protected $table = 'movies';}
```

##### Altres opcions de crear models

El comando anterior **make:model** admet uns paràmetres addicionals, de manera que es pot crear alhora
el model i la migració, i més encara, el model, la migració i el controlador associat. Vegem alguns
exemples:

```php
php artisan make:model Movie -m
```

El comando anterior crea un model **Pelicula** en la carpeta **app\Models** i, a més, crea una
migració anomenada **create_peliculas_table** en la carpeta **database/migrations** , llista perquè
editem el mètode **up** i especifiquem els camps necessaris.
**Notar** que el nom de la migració afig una "s" al nom de la taula automàticament, a partir del model en singular.

```php
php artisan make:model Movie -mc
```

Aquest altre comando crea el mateix que l'anterior, i a més, un controlador anomenat **PeliculaController** en la carpeta **app\Http\Controllers** . Aquest controlador està buit, perquè afegim els mètodes que considerem.

```php
php artisan make:model Movie -mcr
```
Aquesta altra opció crea el mateix que l'anterior, però el controlador **PeliculaController** és en aquest
cas un controlador de recursos, per la qual cosa té ja incorporats el conjunt de mètodes propis d'aquesta
mena de controladors: **index , show , etc**.

#### Seguir una nomenclatura uniforme 

Recorda que, de sessions anteriors, hem comentat la recomanació/necessitat de seguir una nomenclatura uniforme en els models, controladors i vistes. Així, per al **model Movie** ja tindríem el seu controlador associat **MovieController** , i les vistes es definirien en la subcarpetac **resources/views/movies** , amb els noms corresponents a cada mètode del controlador (per exemple **index.blade.php**  o **show.blade.php** ), i amb una taula associada **movies**.

	
##### Clau primària
Laravel també assumeix que cada taula té declarada una clau primària amb el nom **id**. En el cas que no siga així i vulguem canviar-ho haurem de sobreescriure el valor de la propietat protegida **\$primaryKey** del model, per exemple: 

```php
	protected $primaryKey = 'my_id';
```
	
És important definir correctament aquest valor ja que s'utilitza en determinats mètodes de Eloquent, com per exemple per a cercar registres o per a crear les relacions entre models.

##### Timestamps

Una altra propietat que en ocasions haurem d'establir són els timestamps automàtics. Per defecte Eloquent assumeix que totes les taules contenen els camps updated_at i created_at (els quals els podem afegir molt fàcilment amb Schema afegint **\$table->timestamps()** en la migració). Aquests camps s'actualitzaran automàticament quan es creu un nou registre o es modifique. En el cas que no vulguem utilitzar-los (i que no estiguen afegits a la taula) haurem d'indicar-ho en el model o d'una altra forma ens donaria un error. Per a indicar que no els actualitze automàticament haurem de modificar el valor de la propietat pública **\$timestamps** a false, per exemple: 

```php	
	public $timestamps = false;
```
	
A continuació es mostra un exemple d'un model de Eloquent en el qual s'afigen totes les especificacions que hem vist:

```php
	<?php namespace App;
	use Illuminate\Database\Eloquent\Model; 
	class User extends Model
		{
		protected $table = 'my_users'; 
		protected $primaryKey = 'my_id';
		public $timestamps = false;
		}
```	
		
### Ús d'un model de dades

Una vegada creat el model ja podem començar a utilitzar-ho per a recuperar dades de la base de dades, per a inserir noves dades o per a actualitzar-los. 
El lloc correcte on realitzar aquestes accions és en el controlador, el qual li'ls /els hi haurà de passar a la vista ja preparats per a la seua visualització.

És important que per a la seua utilització indiquem a l'inici de la classe l'espai de noms del model o models a utilitzar. 
Per exemple, si anem a usar els models User i Orders hauríem d'afegir:
	
```		
	use App\Models\User;
	use App\Models\Orders;
```		

#### Consultar dades

Per a obtenir totes les files de la taula associada a un model usarem el mètode all():
 
```			use App\Models\Movie;	...	class MovieController extends Controller	{		public function index()		{			$movies = Movie::get();			return view('movies.index', compact('movies'));		}	}	
```	
Aquest mètode pasarà a la vista un array de objectes, on cada item del array serà una instància del model movie i accedirem a les seues propietats com a tals. Així en la vista tindriem alguna cosa com:

```	
@forelse($movies as $movie)	{{ "{{  $movies->titulo " }}}}@endforelse
```	


Alternativament, també podem obtindre una consulta filtrada, amb el mètode **get** i especificant amb el mètode **where** la condició que han de complir els registres a obtindre. Per exemple, així obtindríem els llibres el preu dels quals siga inferior a 10 euros

```	
$movies = Movie::where('precio', '<', 10)->get();
```	
o combinant-les

```	
$movies = Movie::where('precio', '<', 10)->where('precio', '>', 5)->get();
```	

Sobre aquestes consultes base podem aplicar una sèrie d'afegits. Per exemple, podem voler ordenar
els llibres per títol, per al que faríem això en el controlador:

```	php
$movies = Movie::orderBy('titulo', 'DESC')->get();
```	

**Nota**: Tots els mètodes que es descriuen en la secció de "Constructor de consultes" i en la documentació de Laravel sobre "Query Builder" també es poden utilitzar en els models Eloquent. Per tant podrem utilitzar where, orWhere, first, get, orderBy, groupBy, having, skip, take, etc. per a elaborar les consultes.

		
També podem utilitzar els mètodes agregats per a calcular el total de registres obtinguts, o el màxim, mínim, mitjana o summa d'una determinada columna. Per exemple:

```php
	$count = User::where('votes', '>', 100)->count(); 
	$price = Orders::max('price');
	$price = Orders::min('price');
	$price = Orders::avg('price');
	$total = User::sum('votes');
```	

##### Paginaciò de resultats

Si volem paginar els resultats obtinguts, devem, d'una banda, quan obtinguem el llistat des del
controlador, indicar amb **paginate** quants registres volem per pàgina:

```php
public function index()	{		$movies = Movie::paginate(5);		return view('movies.index', compact('moviemovies'));	}
```

Després, en la vista associada ( **movies.index** en l'exemple anterior), podem emprar el mètode
links perquè mostre els botons de paginació en el lloc desitjat:

```php
	@forelse($movies as $movie)		{{ "{{  $movie->titulo " }}}}	@endforelse	{{ "{{  $movies->links() " }}}}
```
###### Paginació des de Laravel 8

En la versió 8 de Laravel s'ha canviat l'estil dels botons de paginació, emprant el del
**framework Tailwind CSS**. Si volem continuar utilitzant els de **Bootstrap**, hem d'afegir aquesta línia en el mètode **boot** del ***provider** **App\Providers\AppServiceProvider** :

```
Paginator::useBootstrap();
```

A més hem d'incorporar la clausula use per localitzar l'element **Paginator**

use Illuminate\Pagination\Paginator;


#### Objectes individuals

Una operació bastant habitual és mostrar una fitxa d'un objecte a partir d'un llistat, fent clic en el
títol o alguna part visible d'aqueix objecte. Per exemple, si volem veure les dades d'un llibre a partir d'un
llistat amb els seus títols, podem fer alguna cosa com això en la plantilla **Blade**:

```php
@forelse($movies as $movie)	<li>
		<a href="{{ "{{  route('movies.show', $movie) " }}}}">		{{ "{{  $movie->titulo " }}}}</a>
	</li>@endforelse
```

Veiem que hem utilitzat el mètode **route** per a indicar la ruta a seguir, amb un segon paràmetre,
que en aquest cas és l'objecte concret d'aqueixa fila. Laravel automàticament ho reemplaçarà en l'enllaç per l'identificador d'aquest objecte.
Per part seua, la ruta associada a aquest enllaç podria ser una cosa així (en l'arxiu de rutes):

```php
Route::get('/movies/{id}', [movieController::class, 'show'])->name('movies.show');
```
#### Mostar dades

Finalment, el mètode show del controlador associat s'encarregarà d'obtindre les dades del llibre a partir de el seu **id**, i generar la vista corresponent. Per a obtindre les dades d'un objecte a partir del seu identificador,podem emprar el mètode **find** del model, passant-li com a paràmetre l'identificador. Així,podríem generar una vista amb les dades com aquesta:

```php
...class movieController extends Controller{	...	public function show($id)	{		$movie = movie::find($id);		return view('movies.show', compact('movie'));	}}
```

En el cas que l'objecte no es trobe (perquè, per exemple, utilitzem un aneu equivocat), la vista
generada fallarà. Per a evitar-ho, en lloc del mètode **find** podem emprar *findOrFail , que, en cas que
   no es trobe l'objecte, generarà una vista amb un error 404, més apropiada. A més,
recorda que pots personalitzar aquestes pàgines d'error definint les vistes corresponents.

```
$movie = movie::findOrFail($id);
```


#### Inserir dades

Per a afegir una entrada en la taula de la base de dades associada amb un model simplement hem de crear una nova instància d'aquest model, assignar els valors que vulguem i finalment guardar-los amb el mètode save():

```php
	$movie = new movie();	$movie->titulo = "La guerra de las galaxias";	$movie->director = "George Lucas";	$movie->precio = 3.95;	$movie->save();
```

Per a obtenir l'identificador assignat en la base de dades després de guardar (quan es tracte de taules amb índex auto-incremental), ho podrem recuperar simplement accedint al camp id de l'objecte que havíem creat, per exemple:

 	$insertedId = $movie->id;
 	
Com a alternativa, també es pot utilitzar el mètode **create** del model, i passar-li totes les dades de la
petició, que arribarien des d'un formulari, com veurem més endavant: 	

```php
movie::create($request->all());
```
Perquè això últim funcione, han de complir-se dues premisses:

* Cada camp de la petició ha de tindre associat un camp del mateix nom en el model.

* Hem de definir en el model una propietat anomenada **\$fillable** amb els noms dels camps
de la petició que ens interessa processar (la resta es descarten). Això és obligatori especificar-ho,
encara que ens interessen tots els camps, per a evitar insercions massives malintencionades (per exemple
, editant el codi font per a afegir altres camps i modificar dades inesperades).

```php
class movie extends Model{protected $fillable = ['titulo', 'director', 'precio'];}
```

Aquest codi d'inserció (o bé camp a camp, o usant el mètode **all** ) se sol posar en el mètode
**store** del controlador, perquè reba les dades del formulari d'inserció i la faça en la base de dades
.Ho acabarem de veure quan abordem el tema dels formularis.

#### Modificar dades

La modificació consisteix en dos passos:

* Trobar l'objecte a modificar (buscant-lo per l'id amb **findOrFail()** , per exemple, com s'ha
explicat abans).
* Modificar les propietats que es necessiten, i cridar al mètode **save()** de l'objecte per a guardar els
canvis.

```php
$movieAModificar = movie::findOrFail($id);$movieAModificar->titulo="Otro título";$movieAModificar->save();
```
També podem utilitzar el mètode **update** enllaçat amb **findOrFail** , i passar-li com a paràmetre
totes les dades de la petició, igual que s'ha explicat per a la inserció, i sempre que hàgem
declarat l'atribut **\$fillable** en el model per a indicar quins camps s'accepten:

```
movie::findOrFail($id)->update($request->all());
```
	
#### Esborrar dades

Per a esborrar una instància d'un model en la base de dades simplement hem d'usar el seu mètode delete():

```
	movie::findOrFail($id)->delete();
```	
	
Això ho farem normalment en el mètode **destroy** del controlador en qüestió. Després, podem
redirigir o renderitzar alguna vista resultat, com el llistat de llibres general per a comprovar que s'ha
esborrat.

```php
public function destroy($id){	movie::findOrFail($id)->delete();	$movies = movie::get();	return view('movies.index', compact('movies'));}
```

##### Sobre lesborrat des de les vistes

El normal és que l'esborrat s'active fent clic en algun element d'una vista. Per exemple, fent
clic en un botó o enllaç que pose "Esborrar". Tanmateix, si implementem això així:

```html	
	<a href="{{ "{{  route('movies.destroy', $movie " }}}}">Borrar</a>
```

Si volem esborrar el llibre amb id 3, es generarà una ruta **http://biblioteca/movies/3**. Ho podem comprovar
passant el ratolí per l'enllaç i veient la barra inferior d'estat del navegador. Aquesta ruta, no obstant això,ens enviarà a la fitxa del llibre 3, no a l'esborrat, ja que estem enviant una petició **GET**, i no una d'esborrat (**DELETE**). Per a evitar això, l'opció d'esborrat ha de fer-se sempre des d'un formulari, on a través del **helper** **@method** indiquem que és una petició d'esborrat (DELETE). Amb el que l'enllaç per a esborrar un llibre quedaria així:

```html
<form action="{{ "{{  route('movies.destroy', $movie) " }}}}" method="POST">	@method('DELETE')	@csrf	<button>Borrar</button></form>
```


NOTA el helper @csrf ho veurem amb més detall en parlar de formularis, però s'afig als formularis Laravel per a evitar atacs de tipus cross-site, és a dir, accessos a una URL de la nostra web
des d'altres webs.
	
## Exemple









