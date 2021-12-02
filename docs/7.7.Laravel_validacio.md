# Formularis i validació de dades
	
[Validació de Formularis](#validació-de-formularis)

[Casos Especials](#casos-especials)

[Exercisis](#exercisis)

[Exemple](#videoclub)

	
## [Dades d'entrada](https://laravel.com/docs/8.x/requests#retrieving-input)

Laravel facilita l'accés a les dades d'entrada de l'usuari a través de solament uns pocs mètodes. No importa el tipus de petició que s'haja realitzat (POST, GET, PUT, DELETE), si les dades són d'un formulari o si s'han afegit a la query string, en tots els casos s'obtindran de la mateixa forma.

Per a aconseguir accés a aquests mètodes Laravel utilitza **injecció de dependències**. Açò és simplement afegir la classe **Request** al constructor o mètode del controlador en el qual ho necessitem. Laravel s'encarregarà d'injectar aquesta dependència ja inicialitzada i directament podrem usar aquest paràmetre per a obtenir les dades d'entrada. 

### Creació i enviament de formularis

Si definim un formulari en una vista, es defineix amb els conceptes que ja sabem d'HTML. Com a únic afegit, en el camp **action** del formulari podem utilitzar Blade i la funció **route** per a indicar el nom de ruta a la qual volem enviar el formulari.
Vegem, per exemple, com definir un formulari per a donar d'alta noves pelicules.
Creem una vista anomenada **create.blade.php** en la subcarpeta
**resources/views/catalog** , amb un contingut com aquest:

```html
@extends('plantilla')
@section('titulo', 'Nueva Pelicula')
@section('contenido')
	<h1>Nueva pelicula</h1>
	<form action="{{ route('catalog.store') }}" method='POST'>
        @csrf
            <div class="form-group">
                <label for="title">Título</label>
                <input type="text" name="title" id="title" class="form-control">
            </div>
            <div class="form-group">
                <label for='year'>Any:</label>
                <input type='number' name='year' />
            </div>
            <div class="form-group">
                <label for='director'>Director:</label>
                <input type='text' name='director' />
            </div>
           <div class="form-group">
                <label for='poster'>Poster:</label>
                <input type='url' name='poster' />
            </div>
            <div class="form-group">
                <label for="synopsis">Resumen</label>
                <textarea name="synopsis" id="synopsis" class="form-control" rows="3"></textarea>
            </div>
             <div class="form-group text-center">
                <button type="submit" class="btn btn-primary" style="padding:8px 100px;margin-top:25px;">Afegir pel.lícula</button>
            </div>
    </form>	
@endsection
```

Un segon afegit més que hem de tindre en compte és que Laravel per defecte protegeix d'atacs XSS (Cross Site Scripting) de suplantació d'identitat, per la qual cosa obtindrem un error de tipus 419 si enviem un formulari no validat. Per a solucionar aquest problema, n'hi ha prou amb utilitzar la directiva **@csrf**
en el formulari, que afig un camp ocult amb un **token** de validació de l'usuari.

En qualsevol cas, aquest formulari s'enviarà a la ruta indicada, que serà el mètode store de catalog que s'encarregarà de recollir les dades de la petició a través del paràmetre **Request** d'aquest mètode. Disposem d'un mètode **get** per a accedir a cada camp del formulari a partir del seu nom:

```php
public function store(Request $request)
{
	$movie = new Movie();
	$movie->titulo = $request->get('titulo');
	$movie->director = $request->get('director');
	$movie->year = $request->get('year');
	..
	$movie->save();
	return redirect()->route('movie.index');
}
```

En aquest exemple com es pot veure s'ha afegit la classe Request com a paràmetre al mètode store. Laravel automàticament s'encarrega d'injectar aquestes dependències pel que directament podem usar la variable **$request** per a obtenir les dades d'entrada.

Si el mètode del controlador tinguera més paràmetres simplement els haurem d'afegir a continuació de les dependències, per exemple:

```php
	public function edit(Request $request, $id) {
	//... }
```
	
A continuació veurem els mètodes i dades que podem obtenir a partir de la variable $request.

#### Obtenir els valors d'entrada

Per a obtenir el valor d'una variable d'entrada usem el mètode **input** indicant el nom de la variable: 

```php
	$name = $request->input('nom');
	// O simplement....
	$name = $request->nom;
```
	
També podem especificar un valor per defecte com a segon paràmetre: 

```php
	$name = $request->input('nom', 'Pedro');
```
	
#### Comprovar si una variable existeix
Si ho necessitem podem comprovar si un determinat valor existeix en les dades d'entrada: 

```php
	if ($request->has('nom')) { //...}
```
### Actualitzacions i esborrats

Per defecte, l'atribut **method** d'un formulari només admet els valors GET o POST. Si volem enviar un formulari d'actualització o esborrat, aquest ha d'anar associat als mètodes **PUT** o **DELETE**,respectivament. Per a això, podem emprar dins del mateix formulari la directiva **@method** ,indicant el nom del mètode que volem usar:

```php
<form ...>
	@csrf
	@method('PUT')
	...
</form>
```

## Validació de formularis

A més d'aplicar una validació en el client a través d'HTML5, que també és recomanable, s'han de validar les dades en el servidor. Per a fer això, el propi objecte **request** proporciona un mètode anomenat **validate** , al qual li passem un array amb les regles de validació.
Per exemple, així comprovaríem que el títol i l'editorial s'han enviat, i que el títol té una grandària mínima de 3 caràcters. A més, comprovem que el preu és un valor numèric real positiu.

```php
public function store()
{
	request()->validate(
	[
		'title' => 'required|min:3',
		'director' => 'required',
		'year' => 'required|numeric|min:1900'
	]
);
// ... Código para procesar el formulario
}
```

### Utilitzar form requests per a validacions més complexes

Si hem de validar uns pocs camps, pot ser adequat cridar al mètode **validate** des del propi mètode del controlador, però per a formularis més grans el codi pot créixer massa.
Una alternativa que ofereix laravel és crear un **form request**, una classe addicional que conté la lògica de validació d'una petició. Es creen amb el comando php artisan , i l'opció make:request ,seguida del nom de la classe a crear:

```php
php artisan make:request MoviePost
```

Aquesta classe s'emmagatzema per defecte en **app/Http/Requests** , i conté un parell de mètodes predefinits:
**authorize** : retorna un booleà depenent de si l'usuari actual està autoritzat a enviar la petició o no. Per a molts formularis que no requerisquen autorització prèvia podem simplement retornar **true** . Serà el que farem de moment en aquest formulari.
**rules** : aquest és el mètode que més ens interessa. Retorna un array de regles de validació com les que teníem en el controller, així que movem aqueix codi ací:


```php
public function rules()
{
return [
	'title' => 'required|min:3',
	'director' => 'required',
	'year' => 'required|numeric|min:1900'
];
}
```

Ara, en el mètode del controlador simplement hem d'injectar aquest form request com a paràmetre (si observem la classe que s'ha creat, és un subtipus de Request ), i usar-lo per a validar. La validació és automàtica, és a dir, no hem d'afegir més codi al controlador que l'objecte injectat com a paràmetre,
que s'encarregarà de validar la pròpia petició que conté a través del mètode rules .

```php
public function store(MoviePost $request)
{
// Si entramos aquí, el formulario es válido
}
```

#### Mostrar missatges derror

Si la validació és correcta, es retornarà la dada del final de la funció, però si falla algun camp, es tornarà a la pàgina del formulari, amb la informació de l'error que s'haja produït. Podem accedir des de qualsevol
lloc de Laravel a la variable \$errors amb els errors que s'hagen produït en una
operació determinada. Aquesta variable té un mètode booleà anomenat **any** que comprova si hi ha algun error, i un altre mètode anomenat **all** que retorna el array d'errors produïts. Combinant aquests dos mètodes amb Blade, podem mostrar el llistat d'errors de validació abans del formulari, d'aquesta manera:

```php
@if ($errors->any())
	<ul>
		@foreach($errors->all() as $error)
			<li>{{ $error }}</li>
		@endforeach
	</ul>
@endif
<form ...>
	@csrf
	...
</form>
```

També podem emprar el mètode first del array d'errors per a obtindre el primer error associat a un camp, i mostrar-lo baix o sobre el camp en qüestió. Per exemple:

```php
<form action="{{ route('movies.store') }}" method="POST">
	@csrf
	<div class="form-group">
		<label for="titulo">Título:</label>
		<input type="text" class="form-control" name="titulo"
id="titulo">
		@if ($errors->has('titulo'))
			<div class="text-danger">
				{{ $errors->first('titulo') }}
			</div>
		@endif
	</div>
...
```

A més, podem personalitzar el missatge d'error a mostrar, redefinint en la classe del *form request el mètode **messages** . En aquest mètode retornem un array amb el missatge a mostrar per a cada possible error de validació. Per exemple:

```php
public function messages()
{
	return [
		'titulo.required' => 'El título es obligatorio',
		...
	];
}
```


De manera alternativa, si optem per validar el formulari en el propi controlador, aquest array de missatges
és passa com a segon paràmetre en l'anomenada al mètode **validate** :

```php
request()->validate(
[
	'title' => 'required|min:3',
	'director' => 'required',
	'year' => 'required|numeric|min:1900'
], [
	'titulo.required' => 'El título es obligatorio',
	...
]
);
```

#### Recordar valors enviats

Un problema derivat de la validació de dades és que, en tornar a la pàgina del formulari després d'un error, els camps que ja s'han examinat fins a l'error, encara que foren correctes, han perdut el valor que tenien, i pot resultar enutjós haver-los de que emplenar una altra vegada. Per a mantindre el seu antic valor, podem afegir l'atribut **value** en cada camp del formulari, i utilitzar amb Blade una funció anomenada **old** , que permet accedir a l'anterior valor d'un determinat camp, referenciat pel seu nom:

```
<form action="{{ route('movies.store') }}" method="POST">
	@csrf
	<div class="form-group">
		<label for="titulo">Título:</label>
		<input type="text" class="form-control" name="titulo"
			id="titulo" value="{!! old('titulo') !!}">
		@if ($errors->has('titulo'))
			<div class="text-danger">
				{{ $errors->first('titulo') }}
			</div>
		@endif
	</div>
...
```

## Casos especials

#### Obtenir dades agrupades

O també podem obtenir totes les dades d'entrada alhora (en un array) o solament alguns d'ells:

```php
	// Obtenir tots:
		$input = $request->all();
	// Obtenir solament els camps indicats:
		$input = $request->only('username', 'password');
	// Obtenir tots excepte els indicats: 
		$input = $request->except('credit_card');
```

#### Assignació Massiva 

També es pot utilitzar el mètode **create** per a emmagatzemar un model en una única línia. Des del mètode es retornarà la instància del model inserit. No obstant açò, abans d'açò, cal especificar la propietat **fillable** o **guarded** del model, doncs tots els models Eloquent posseeixen protecció contra l'assignació en massa.

Una vulnerabilitat d'assignació massiva té lloc quan un usuari passa un paràmetre HTTP inesperat a través de la sol·licitud, i aquest paràmetre canvia una columna de la base de dades que no s'esperava. Per exemple, un usuari malintencionat podria enviar un paràmetre **is_admin** a través d'una petició HTTP, el qual es marejaria dins del mètode **create** del model, permetent a l'usuari postular-se com un administrador.

Així que, per a començar, cal definir a quins atributs se'ls permet l'assignació massiva. Açò s'estableix en la propietat **fillable** del model. Per exemple, anem a permetre l'assignació massiva sobre l'atribut **name** d'un model Flight:

	<?php
	
	namespace App;
	
	use Illuminate\Database\Eloquent\Model;
	
	class Flight extends Model
	{
	    /**
	     * The attributes that are mass assignable.
	     *
	     * @var array
	     */
	    protected $fillable = ['name'];
	}

##### Atributes Guarding

Mentre que \$fillable serveix com una "llista blanca" d'atributs que poden ser assignats massivament, també es pot optar per \$guarded. La propietat guarded conté un array d'atributs que no poden ser assignats de forma massiva. 
La resta d'atributs que no es troben en el array si podran. Pel que, \$guarded actua com una "llista negra". Per descomptat, s'ha d'establir \$fillable o \$guarded - mai tots dos. En l'exemple que segueix, tots els atributs excepte **price** comptaran amb assignació massiva:

	<?php
	
	namespace App;
	
	use Illuminate\Database\Eloquent\Model;
	
	class Flight extends Model
	{
	    /**
	     * The attributes that aren't mass assignable.
	     *
	     * @var array
	     */
	    protected $guarded = ['price'];
	}

Si es desitja que tots els atributs es puguen assignar en massa, es pot definir la propietat $guarded com un array buit:

	/*
	* The attributes that llauren't mass assignable.
	*
	* @var array
	/
	protected $guarded = [];
	
#### Fitxers d'entrada

Laravel facilita una sèrie de classes per a treballar amb els fitxers d'entrada. Per exemple per a obtenir un fitxer que s'ha enviat en el camp amb nom photo i guardar-ho en una variable, hem de fer:

	$file = $request->file('photo');
	// O simplement... 
	$file = $request->photo;
	
Si volem podem comprovar si un determinat camp té un fitxer assignat:
	
	if ($request->hasFile('photo')) { //...}

El Symfony\Component\HttpFoundation\File\UploadedFile estén de la classe de PHP [SplFileInfo](http://php.net/manual/es/class.splfileinfo.php), per tant, tindrem molts mètodes que podem utilitzar per a obtenir dades del fitxer o per a gestionar-ho.

Per exemple, per a comprovar si el fitxer que s'ha pujat és vàlid:

	if ($request->file('photo')->isValid()) { //...}

O per a moure el fitxer d'entrada a una ruta determinada: 

	// Moure el fitxer a la ruta conservant el nom original
		$request->file('photo')->move($destinationPath);
	// Moure el fitxer a la ruta amb un nou nom: 
		$request->file('photo')->move($destinationPath, $fileName);
		
Laravel incorpora una llibreria que ens permet gestionar l'accés i escriptura de fitxers en un [emmagatzematge](https://laravel.com/docs/8.x/filesystem). L'interessant d'açò és que ens permet manejar de la mateixa forma l'emmagatzematge en local, en Amazon S3 i en Rackspace Cloud Storage, simplement ho hem de configurar en config/filesystems.php i posteriorment els podrem usar de la mateixa forma. 

Per exemple, per a emmagatzemar un fitxer pujat mitjançant un formulari hem d'usar el mètode store indicant com a paràmetre la ruta on volem emmagatzemar el fitxer (sense el nom del fitxer):

```php
$path = $request->photo->store('images');
$path = $request->photo->store('images', 's3'); // Especificar un emmagatzematge
```

Aquests mètodes retornaran el path fins al fitxer emmagatzemat de forma relativa a l'arrel de disc configurada. Per al nom del fitxer es generarà automàticament un UUID (identificador únic universal). Si volem especificar nosaltres el nom hauríem d'usar el mètode storeAs:

	$path = $request->photo->storeAs('images', 'filename.jpg'); 
	$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
	
Altres mètodes que podem utilitzar per a recuperar informació del fitxer són:

```php
	// Obtenir la ruta:
	$path = $request->file('photo')->getRealPath();
	// Obtenir el nom original:
	$name = $request->file('photo')->getClientOriginalName();
	// Obtenir l'extensió:
	$extension = $request->file('photo')->getClientOriginalExtension();
	// Obtenir la grandària:
	$size = $request->file('photo')->getSize();
	// Obtenir el Type:
	$acarone = $request->file('photo')->getMimeType();
```