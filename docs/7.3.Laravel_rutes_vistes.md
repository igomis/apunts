# FRAMEWORK LARAVEL

## Rutes
[![](../img/ull.png)Video](https://youtu.be/oHJokaq0yeY)

Podríem dir que existeixen dos tipus principals de rutes:

* Les rutes **web** (emmagatzemades a l'arxiu **web.php** de l'aplicació), que ens permetran carregar diferents vistes en funció de la URL que indique el client.

* Les rutes **API** (emmagatzemades en l'arxiu **api.php**), a través de les quals deﬁnirem
diferents serveis **REST**, com veurem també més endavant.

Ens centrarem durant aquesta tema en el primer grup, per la qual cosa editarem el contingut de l'arxiu **routes/web.php**. Vegem quins tipus de rutes podem *deﬁnir, i quines característiques tenen.

Este és el punt centralitzat per a la definició de rutes i qualsevol ruta no definida en aquest fitxer no serà vàlida, generat una excepció (el que retornarà un error 404).
Les rutes, en la seua forma més senzilla, poden retornar directament un valor des del propi fitxer de rutes, però també podran generar la trucada a una vista o a un controlador.

#### Rutes simples

Les rutes simples tenen un nom de ruta fix, i una funció que respon a aquest nom emetent una resposta.
Les rutes, a més de definir la URL de la petició, també indiquen el mètode amb el qual s'ha de fer aquesta petició. Els dos mètodes més utilitzats i que començarem veient són les peticions tipus GET i tipus POST. Per exemple, per a definir una petició tipus GET hauríem d'afegir el següent codi al nostre fitxer routes.php:
  
```php  
	Route::get('salut', function()
	{ 
		return 'Hola món!'; 
	});
```

Aquest codi es llançaria quan es realitze una petició tipus GET a la ruta arrel de la nostra aplicació. Si estem treballant en local aquesta ruta seria http://localhost (en el notre cas **intranet.my**) però quan la web estiga en producció es referiria al domini principal, per exemple: http://www.dirección-de-tu-web.com. 
   

#### Afegir paràmetres a les rutes

Si volem afegir paràmetres a una ruta simplement els hem d'indicar entre claus {} a continuació de la ruta, de la forma:
 
```php    
    Route::get('salut/{nom}', function($nom)
    {
	return 'Bon dia, '.$nom; });
```    

En aquest cas estem definint la ruta /salut/{nom}, on **nom** és requerit i pot ser qualsevol valor. En cas de no especificar cap **nom** ens redigirirà a una pàgina d'error 404. El paràmetre se li passarà a la funció, el qual es podrà utilitzar (com veurem més endavant) para per exemple obtenir dades de la base de dades, emmagatzemar valors, etc.

També podem indicar que un paràmetre és opcional simplement afegint el símbol ? al final (i en aquest cas no donaria error si no es realitza la petició amb aquest paràmetre):

```php     
    Route::get('salut/{nom?}', function($nom = 'convidat')
    {
    	return 'Bon dia, '.$nom; 
	});
```   
#### Validació de paràmetres  
    
Alguns paràmetres caldrà que seguisquen un determinat patró. Per exemple, un identificador numèric
només contindrà dígits. Per a assegurar-nos d'això, podem emprar el mètode *where en definir la ruta. A aquest mètode li passem dos paràmetres: el nom del paràmetre a validar, i l'expressió regular que
ha de complir. En el cas del nom anterior, si volem que només continga lletres (majúscules o minúscules), podem fer una cosa així:    

```php 
Route::get('salut/{nom?}', function($nom = "Convidat") {
return "Hola, " . $nom;
})->where('nom', "[A-Za-z]+");
```
En cas que la ruta no complisca el patró, s'obtindrà una pàgina d'error. Més endavant s'explicarà
com podem personalitzar aquestes pàgines d'error.

#### Named routes

A vegades pot ser convenient associar un nom a una ruta. Especialment, quan aqueixa ruta formarà part d'un enllaç en alguna pàgina del nostre lloc, ja que en un futur la ruta podria canviar, i d'aquesta manera evitem haver d'actualitzar els enllaços al nou nom.
Per a això, en definir la ruta, li associem amb la funció **name** el nom que vulguem. Per exemple: 

```php 
Route::get('contacte', function() {
return "Pàgina de contacte";
})->name('ruta_contacte');
```
Ara, si volem definir un enllaç a aquesta ruta en qualsevol part, n'hi ha prou amb emprar la funció **route** de Laravel, indicant el nom que li hem assignat a aquesta ruta. Per tant, en lloc de posar això:

```php 
echo '<a href="/contacte">Contacte</a>';
```
Podem fer alguna cosa com això un altre, tal com veurem a continuació quan usem el motor de plantilles **Blade**:

```php 
<a href="{{ "{{ route('ruta_contacte') " }}}}">Contacte</a>
```
D'aquesta manera, davant futurs canvis en les rutes, només haurem de canviar la URL en **Route::get**

#### Combinació d'elements en rutes

Podem combinar diverses clàusules **where** en una ruta per a validar diferents paràmetres que puga
tindre, i també enllaçar aquestes crides amb una a la funció **name** per a nomenar la ruta. Per exemple, la
següent ruta espera rebre un nom amb caràcters, i un id numèric, tots dos amb valors per defecte:

```php
Route::get('salut/{nom?}/{id?}',
function($nom="Convidat", $id=0)
{
return "Hola $nom, el teu codi és el $id";
})->where('nom', "[A-Za-z]+")
->where('id', "[0-9]+")
->name('salut');
```

Si accedim a cadascuna de les següents URLs, obtindrem cadascuna de les respostes indicades:

|URL|Respuesta|
|--|--|
|/salut| Hola Convidat, el teu codi és el 0|
|/salut/Ignasi| Hola Ignasi, el teu codi és el 0|
|/salut/Ignasi/3| Hola Ignasi, el teu codi és el 3|
|/salut/3| Error 404 (URL incorrecta)|

Notar que l'últim cas és incorrecte. No podem especificar un id sense haver especificat un nom
davant, perquè incompleix el patró de la URL. Es pot deixar un paràmetre omés, sempre que els
posteriors també ho estiguen.


## Vistes

[![](../img/ull.png)Video](https://youtu.be/D3uLYwkQ3C0)

Les vistes són la forma de presentar el resultat (una pantalla del nostre lloc web) de forma visual a l'usuari, el qual podrà interactuar amb ell i tornar a realitzar una petició. Les vistes a més ens permeten separar tota la part de presentació de resultats de la lògica (controladors) i de la base de dades (models).

Per tant no hauran de realitzar cap tipus de consulta ni processament de dades, simplement rebran dades i els prepararan per a mostrar-los com HTML.

Fins ara les rutes que hem definit retornen un text simple, excepte la qual ja estava creada per defecte
en el projecte, que apuntava a la pàgina d'inici. Si volguérem retornar contingut HTML, una
opció (costosa) seria retornar aquest contingut generat des del propi mètode de la ruta, a través de la
instrucció **return** , però en lloc de fer això des de dins de la pròpia funció de resposta, el més
habitual (i recomanable) és generar una vista amb el contingut HTML que es vol enviar al client.

La forma general de mostrar vistes en **Laravel** és fer que les rutes retornen (return) una determinada vista. Per a això, es pot emprar la funció **view** de Laravel, indicant el nom de la vista a generar o mostrar.
Per defecte, en la carpeta **resources/views** tenim disponible una vista d'exemple anomenada **welcome.blade.php** . És la que s'utilitza com a pàgina d'inici en la ruta arrel en **routes/web.php** :

```php
Route::get('/', function() {
return view('welcome');
});
```
Notar que no és necessari indicar el path o ruta cap a l'arxiu de la vista, ni tampoc l'extensió, ja que
 Laravel assumeix que per defecte les vistes es troben en la carpeta **resources/views** , amb l'extensió
**.blade.php** (que fa referència al motor de plantilles **Blade** que veurem a continuació), o
simplement amb extensió **.php** (en el cas de vistes simples que no utilitzen Blade).
Podem, per exemple, crear una vista senzilla dins d'aquesta carpeta de vistes (anomenem-la
**inici.blade.php** ), amb un contingut HTML bàsic:

```html
<html>
	<head>
		<title>Inici</title>
	</head>
	<body>
		<h1>Pàgina d'inici</h1>
	</body>
</html>
```
I podem utilitzar esta vista com a pàgina d'inici:

```php
Route::get('/', function() {
return view('inicio');
});
```

Les vistes contenen el codi HTML del nostre lloc web, barrejat amb els **assets** (CSS, imatges, Javascripts, etc. que estaran emmagatzemats en la carpeta public) i una **mica de codi PHP (o codi Blade de plantilles, com veurem més endavant)** per a presentar les dades d'entrada com un resultat HTML.

##### Pasar valor a les vistes

És molt habitual passar una certa informació a unes certes vistes, com per exemple, llistats de dades a mostrar, o dades d'un element en concret. Per exemple, si volem donar un missatge de benvinguda a un nom
(suposadament variable), hem d'emmagatzemar el nom en una variable en la ruta, i passar-s'ho a la vista en carregar-la. Això pot fer-se, per exemple, amb el mètode **with** després de generar la vista, indicant el nomamb què l'associarem a la vista, i el valor (variable) associat a aquest nom. En el nostre cas quedaria així:

```php
Route::get('/', function() {
$nombre = "Nacho";
return view('inicio')->with('nombre', $nombre);
});
```

Posteriorment, en la vista, haurem de mostrar el valor d'aquesta variable en algun lloc del codi HTML.
Podem emprar PHP tradicional per a recollir aquesta variable:

```html
<html>
	<head>
		<title>Inicio</title>
	</head>
	<body>
		<h1>Página de inicio</h1>
		<p>Bienvenido/a <?php echo $nombre; ?></p>
	</body>
</html>
```

Però és més habitual i net emprar una sintaxi específica de Blade, com veurem a continuació.
Com a alternatives a l'ús de with comentat abans, també podem utilitzar un array associatiu (assignant així diversos noms a diversos valors):

```php
return view('inicio')->with(['nombre' => $nombre, ...]);
```

Així mateix, podem utilitzar aquest mateix array com segon paràmetre de la funció view , i prescindir
així de with :

```php
return view('inicio', ['nombre' => $nombre, ...]);
```

I també podem utilitzar una funció anomenada compact com segon paràmetre de view . A aquesta funció li passem únicament el nom de la variable i, sempre que la variable associada es cride igual, estableix l'associació per nosaltres:

```php
return view('inicio', compact('nombre'));
```

La funció **compact** admet tants paràmetres com dades vulguem enviar a la vista per separat, cadascun amb el seu nom associat. Si simplement retornarem una vista amb poca informació associada, o poca lògica interna, també podem abreujar el codi anterior cridant directament a view , en lloc de route primer, en
l'arxiu **routes/web.php** , i li passem així la informació associada a la vista:

```php
Route::view('/', 'inicio', ['nombre' => 'Nacho']);
```
         
## [Plantilles mitjançant Blade](https://laravel.com/docs/8.x/blade)

Laravel utilitza Blade per a la definició de plantilles en les vistes. Aquesta llibreria permet realitzar tot tipus d'operacions amb les dades, a més de la substitució de seccions de les plantilles per un altre contingut, herència entre plantilles, definició de layouts o plantilles base, etc.

Els fitxers de vistes que utilitzen el sistema de plantilles Blade han de tenir l'extensió **.blade.php**. Aquesta extensió tampoc s'haurà d'incloure a l'hora de referenciar una vista des del fitxer de rutes o des d'un controlador. És a dir, utilitzarem view('home') tant si el fitxer es diu home.php com home.blade.php.

En general el codi que inclou Blade en una vista començarà pels símbols **@** o **{{ "{{**, el qual posteriorment serà processat i preparat per a mostrar-se per pantalla. Blade no afig sobrecàrrega de processament, ja que totes les vistes són preprocesadas i cachejades, per contra ens brinda utilitats que ens ajudaran en el disseny i modularització de les vistes.

#### Mostrar dades
El mètode més bàsic que tenim en Blade és el de mostrar dades, per a açò utilitzarem les claus dobles (**{{ "{{ " }}}}**) i dins d'elles escriurem la variable o funció amb el contingut a mostrar:

```   
    Hola {{ "{{ $name " }}}}.
```

Cada vegada que es renderitza una vista en Laravel, s'emmagatzema el contingut PHP generat en
**storage/framework/views** , i només es torna a re-generar davant un canvi en la vista, amb el que
tornar a cridar a una vista ja renderitzada no afecta al rendiment de l'aplicació. Si donem una ullada
 a la vista generada amb PHP pla i amb Blade, veurem que hi ha una subtil diferència entre
ambdues, i és que amb Blade, en lloc de fer un simple echo per a mostrar el valor de la variable, s'utilitza
una funció intermèdia anomenada e , que evita atacs **XSS (Cross Site Scripting)**, és a dir, que
s'injecten scripts de Javascript amb la variable a mostrar. En altres paraules, el codi no s'interpreta,
i es mostra tal qual. En alguns casos (especialment quan generem contingut HTML des de dins de l'expressió Blade) ens pot interessar que no protegisca contra aquestes injeccions de codi.
En aqueix cas, se substitueix la segona clau per una doble exclamació:

```   
    Hola {!! $name !!}.
```

En general sempre haurem d'usar les claus dobles, especialment si anem a mostrar dades que són proporcionats pels usuaris de l'aplicació. Per tant, aquest últim mètode solament hem d'utilitzar-ho si estem segurs que no volem que s'escape el contingut.

##### Mostrar una dada sola si existeix
Per a comprovar que una variable existeix o té un determinat valor podem utilitzar l'operador ternari de la forma:

```php    
    {{ "{{ isset($name) ? $name : 'Valor per defecte' " }}}}
```
    
O simplement usar la notació que inclou Blade per a aquesta fi: 

```  
    {{ "{{ $name or 'Valor per defecte' " }}}}
```
  
#### Comentaris
Per a escriure comentaris en Blade s'utilitzen els símbols **{{ "{{-- i --" }}}}**

#### Estructures de control

Per a iterar sobre un conjunt de dades (array), podem emprar la directiva @foreach , amb una sintaxi similar al foreach de PHP, però sense necessitat de claus. N'hi ha prou amb finalitzar el bucle amb la directiva
@endforeach , d'aquesta manera:

```html
<ul>
	@foreach($elementos as $elemento)
		<li>{{ "{{ $elemento " }}}}</li>
	@endforeach
</ul>
```

En el cas de voler realitzar alguna comprovació (per exemple, si el array anterior està buit, per a mostrar
un missatge pertinent), usem la directiva @if , tancada pel seu corresponent parella @endif .
Opcionalment, es pot intercalar una directiva @else per al camí alternatiu, o també @elseif per a indicar una altra condició. L'exemple anterior podria quedar així:

```
<ul>
	@if($elementos)
		@foreach($elementos as $elemento)
			<li>{{ "{{ $elemento " }}}}</li>
		@endforeach
	@else
			<li>No hay elementos que mostrar</li>
	@endif
</ul>
```

També podem comprovar si una variable està definida. En aquest cas, reemplacem la directiva @if per @isset, amb el seu corresponent tancament @endisset.

```html
<ul>
	@isset($elementos)
		@foreach($elementos as $elemento)
			<li>{{ "{{ $elemento " }}}}</li>
		@endforeach
	@else
		<li>No hay elementos que mostrar</li>
	@endisset
</ul>
```

No obstant això, amb qualsevol d'aquestes opcions tenim un problema: en el primer cas, si la variable **\$elements** no està definida, mostrarà un error de PHP. En el segon cas, si la variable sí que està definida però no conté elements, no es mostrarà res per pantalla. Una tercera estructura alternativa que agrupa aquests dos casos (controlar alhora que la variable estiga definida i tinga elements) és emprar la directiva @forelse en lloc de @foreach . Aquesta directiva permet una clàusula addicional @empty per a indicar què fer si la col·lecció no té elements o està sense definir. L'exemple anterior quedaria ara així d'abreujat:

```
<ul>
	@forelse($elementos as $elemento)
		<li>{{ "{{ $elemento " }}}}</li>
	@empty
		<li>No hay elementos que mostrar</li>
	@endforelse
</ul>
```


En aquesta mena de iteradors ( @foreach o @forelse ), tenim disponible un objecte anomenat \$loop ,
amb una sèrie de propietats sobre el bucle que estem iterant, com per exemple index (posició
dins del array per la qual anem), o count (total d'elements), o first i last (booleans que
determinen si és el primer o últim element, respectivament), entre altres. Podem veure totes les
propietats disponibles en aquest objecte cridant a var_dump :

```html
<ul>
	@forelse($elementos as $elemento)
		<li>{{ "{{ $elemento " }}}} {{ "{{ var_dump($loop) " }}}} </li>
	@empty
		<li>No hay elementos que mostrar</li>
	@endforelse
</ul>
```

Si, per exemple, volem determinar si és l'últim element de la llista, i mostrar un missatge o estil especial, podem fer alguna cosa com això:

```html
<ul>
	@forelse($elementos as $elemento)
		<li>{{ "{{ $elemento " }}}}
			{{ "{{ $loop->last ? "Ultimo elemento" : "" " }}}}
		</li>
	@empty
		<li>No hay elementos que mostrar</li>
	@endforelse
</ul> 
```   

Aquesta són les estructures de control més utilitzades. Ademas d'aquestes Blade defineix algunes més que podem veure directament en la seua [documentació](https://laravel.com/docs/8.x/blade#control-structures)


##### Sobre els enllaços a altres rutes

Hem comentat breument en punts anteriors que, gràcies a Blade i als noms en les rutes,podem enllaçar una vista amb una altra de dues formes: de manera tradicional...

```php
echo '<a href="/contacto">Contacte</a>';
```
... o bé emprant la funció route seguida del nom que li hem donat a la ruta:

```php
<a href="{{ "{{ route('ruta_contacto') " }}}}">Contacte</a>
```

A més, mitjançant Blade existeix una tercera manera d'enllaçar, emprant la funció url , que genera una
URL completa fins a la ruta que indiquem:

```php
<a href="{{ "{{ url('/contacto') " }}}}">Contacte</a>
```

### Definir Plantilles comunes

A l'hora de donar homogeneïtat a una web, és habitual que la capçalera, el menú de navegació o el peu de pàgina formen part d'una plantilla que es repeteix en totes les pàgines del lloc, de manera que evitem
haver d'actualitzar totes les pàgines davant qualsevol possible canvi en aquests elements.
Per a crear una plantilla en Blade, creem un arxiu normal i corrent (per exemple,
**plantilla.blade.php** ), en la carpeta de vistes, amb el contingut general de la plantilla. En aquelles zones del document on permetrem contingut variable depenent de la vista en si, afegim
una secció anomenada **@yield** , amb un nom associat. La nostra plantilla podria ser aquesta (notar que es permeten varies **@yield** amb diferents noms):

```html
<html>
	<head>
		<title>
			@yield('titulo')
		</title>
	</head>
	<body>
		<nav>
			<!-- ... Menú de navegación -->
		</nav>
		@yield('contenido')
	</body>
</html>
```
Després, en cada vista en què vulguem utilitzar aquesta plantilla, afegim la directiva @*extends de Blade,
indicant el nom de plantilla que utilitzarem. Amb la directiva @section , seguida del nom de la
secció, definim el contingut per a cadascun dels @yield que s'hagen indicat en la plantilla.
Finalitzarem cada secció amb la directiva @endsection . Així, per a la nostra pàgina inicial
( inici.blade.php ), el contingut pot ser ara aquest:

```html
@extends('plantilla')
@section('titulo', 'Inicio')
@section('contenido')
	<h1>Página de inicio</h1>
	Bienvenido/a {{ "{{ $nombre " }}}}
@endsection
```
Notar, a més, que a la directiva @section se li pot passar un segon paràmetre amb el contingut
d'aqueixa secció, i en aquest cas no és necessari tancar-la amb @endsection . Aquesta opció és útil per a continguts on no interessen caràcters en blanc o salts de línia innecessaris al principi o al final,
com ocorre en l'exemple anterior amb el títol (title) de la pàgina.
De la mateixa manera, la nostra vista per al llistat de llibres quedaria d'aquesta manera:

```html
@extends('plantilla')
@section('titulo', 'Listado de libros')
@section('contenido')
	<h1>Listado de libros</h1>
		<ul>
		@forelse ($libros as $libro)
			<li>{{ "{{ $libro["titulo"] " }}}}({{ "{{ $libro["autor"] " }}}})</li>
		@empty
			<li>No se encontraron libros</li>
		@endforelse
		</ul>
@endsection
```
##### Incloure vistes dins daltres

També sol ser habitual definir continguts parcials (se solen definir en una subcarpeta **partials**
dins de resources/views ), i incloure'ls en les vistes. Per a això, utilitzarem la directiva **@include**
de Blade.
Per exemple, definirem un menú de navegació. Suposem que aquest menú està en l'arxiu
**resources/views/partials/nav.blade.php**.

```html
<nav>
	<a href="{{ "{{ route('inici') " }}}}">Inici</a>
	<a href="{{ "{{ route('libres_llistat') " }}}}">Llistat de llibres</a>
</nav>
```

Per a incloure el menú en la plantilla anterior, podem fer això (i eliminaríem el menú <nav> de la
plantilla):

```html
<html>
	<head>
		<title>
			@yield('titulo')
		</title>
	</head>
	<body>
		@include('partials.nav')
		@yield('contenido')
	</body>
</html>
```

##### Estructuras vistes en carpetes

Quan l'aplicació és una mica complexa, poden ser necessàries diverses vistes, i tindre-les totes en una mateixa carpeta pot ser una cosa difícil de gestionar. És habitual, com anirem veient en sessions posteriors,
estructurar les vistes de la carpeta **resources/views** en **subcarpetas**, de manera que, per exemple, cada carpeta es referisca a les vistes d'una entitat o model de l'aplicació, o a un controlador específic. Per renderitzar una vista que està dins d'una carpeta,  haurem d'indicar també el nom de la subcarpeta:

```php
Route::get('llistat', function() {
	...
	return view('llibres.llistat', compact('llibres'));
});
```

Ací tindrem una vista llistat dins d'una carpeta llibres.


##### Vistes per a pàgines d'error

Quan programem, algunes accions que fem provocaran pàgines d'error amb determinats
codis, com per exemple 404 per a pàgines no trobades. Si volem definir l'aspecte i estructura d'aquestes
pàgines, n'hi ha prou amb crear la vista corresponent en la carpeta **resources/views/errors** , per exemple
, **resources/views/errors/404.blade.php** per a l'error 404 (anteposem el codi d'error
al sufix de la vista).

```php
@extends('plantilla')
@section('titulo', 'Error 404')
@section('contenido')
	<h1>Error</h1>
	Documento no encontrado
@endsection
```

## Enllaçant amb CSS i Javascript en el client

Ara que ja tenim una visió bastant completa del que el motor de plantilles Blade pot oferir-nos, arriba el moment d'acabar de perfilar les nostres vistes. Fins ara no hem parlat res d'estils CSS, i això és una cosa que tota vista que es pree ha d'incloure. A més, també pot ser necessari en alguns casos incloure alguna llibreria Javascript en el costat del client per a uns certs processaments.
Veurem com gestiona Laravel aquests recursos.


#### Infraestructura per a arxius CSS i Javascript.

Per a poder afegir estils CSS o arxius Javascript al nostre projecte Laravel, el framework proporciona
ja uns arxius on centralitzar aquestes opcions.
En primer lloc, hem de tindre en compte que totes les dependències de llibreries en la part del client se centralitzen en l'arxiu **package.json** , disponible en l'arrel del projecte. Inicialment compta ja amb
una sèrie de dependències pre-afegides. Algunes d'elles són importants, com **laravel-mix** , i altres
pot ser que no les necessitem i les puguem esborrar. És recomanable instal·lar les dependències quan
creguem el projecte, per a tindre-les disponibles, amb aquest comando:

```
npm install
```

Aquesta carpeta és similar a la carpeta **vendor** , també en l'arrel del projecte, però aquesta última conté dependències PHP (no Javascript). Cap d'aquestes carpetes ha de pujar-se a un repositori git, ja que
ambdues poden reconstruir-se amb el corresponent comando d'instal·lació de npm o de composer, segons
el cas, i a més, poden ocupar molt d'espai.
A més, d'una banda, tenim l'arxiu **resources/css/app.css** , o bé
**resources/sass/app.scss** (depenent de la versió de Laravel que usem), on podem definir estils CSS propis, o incorporar llibreries externes com veurem després, utilitzant o bé CSS pla o bé Sass. 

D'altra banda, tenim l'arxiu *resources/*js/app.*js per a incloure les nostres pròpies funcions en
Javascript, o fins i tot funcionalitats externes (a través de **jQuery**, per exemple).

#### Generació automàtica de css i javascript

Aquests dos arxius necessiten ser processats per a generar el codi resultant (CSS i Javascript) que
formarà part de l'aplicació, conjuminant totes les llibreries i funcions que hàgem especificat. Per a això,es té l'arxiu **webpack.mix.js** en l'arrel del projecte, que empra l'eina WebPack per a compilar,
empaquetar i minificar aquests arxius resultat CSS i Javascript.

```
mix.js('resources/js/app.js', 'public/js')
.css('resources/sass/app.scss', 'public/css');
```

Com podem intuir, des d'aquest arxiu **webpack.mix.js** es prendrà tot el que hi ha en l'arxiu
**resources/js/app.js** i es generarà un arxiu optimitzat situat en **public/js/app.js** . De manera
similar, es prendran els estils definits en **resources/sass/app.scss o en
**resources/css/app.css (depenent de la versió de Laravel) i es generarà un arxiu **CSS**
optimitzat en **public/css/app.css** . Per a desencadenar aquest procés, Laravel i WebPack es valen de la llibreria **laravel-mix** , inclosa en l'arxiu **package.json** . Per això és important aquesta llibreria, i per això hem de deixar-la instal·lada prèviament amb el comando **npm install** que hem explicat abans. Una vegada instal·lada, per a generar els CSS i Javascript hem d'executar aquest comando des de l'arrel del projecte:

```
npm run dev
```


Això generarà els arxius **public/css/app.css** i **public/js/app.js** , i després ja podrem
afegir aquests arxius en les nostres vistes, amb alguna cosa com això, respectivament:

```html
<html>
	<head>
		<link rel="stylesheet" type="text/css" href="/css/app.css">
	<script type="text/javascript" src="/js/app.js">
	</script>
```

#### Incloure estils Bootstrap

Un dels frameworks de disseny web més utilitzats a l'hora d'elaborar una web és Bootstrap. En aquest
curs no donarem massa nocions sobre ell, però sí que utilitzarem algunes pinzellades perquè les nostres vistes tinguen un aspecte més professional.
Per a incloure aquest framework en Laravel, hem d'incloure una llibreria en el servidor anomenada ui, que s'encarrega d'incorporar diferents eines per a disseny d'interfícies d'usuari (UI, User Interface).

```
composer require laravel/ui
```

Una vegada afegida l'eina, la podem emprar a través del comando artisan per a incorporar
Bootstrap al projecte:

```
php artisan ui bootstrap
```

Això incorporarà Bootstrap a l'arxiu package.json , en la secció de dependències...

```
"devDependencies": {
...
"bootstrap": "^4.0.0",
...
}
```
... i també afegirà un enllaç a aquesta llibreria en l'arxiu **resources/sass/app.scss** , perquè
puguem generar un arxiu CSS optimitzat amb Bootstrap inclòs: 

```
...
@import '~bootstrap/scss/bootstrap';
```


Per a poder utilitzar finalment Bootstrap, hem d'executar novament les instruccions:

```
npm install
npm run dev
```


La primera instrucció descarregarà i instal·larà Bootstrap en el projecte (en la subcarpeta node_modules), ila segona generarà els arxius CSS i Javascript incloent en ells la llibreria Bootstrap. Amb això ja tindrem disponibles les classes i estils de Bootstrap per a les nostres vistes.
 
## Proves amb laravel
En les primeres rutes de la nostra aplicació, utilitzem el navegador per a provar aquestes rutes i URLs. El problema d'aquestes proves en el navegador és que no perduren en el temps ni poden executar-se de forma ràpida / automàtica. Així que avui veurem com podem provar el codi que desenvolupem de forma més intel·ligent, utilitzant el component de proves automatitzades que ve inclòs amb Laravel.

##### Directori de proves
Laravel inclou en el directori principal del teu projecte un directori anomenat /tests. En aquest directori anem a escriure codi que es va a encarregar de provar el codi de la resta de l'aplicació. Aquest directori està separat en dos subdirectoris:

* El directori Feature on escrivim proves que emulen peticions HTTP al servidor.
* El directori Unit on escrivim proves que s'encarreguen de provar parts individuals de l'aplicació (com a classes i mètodes).

##### Escrivint una prova
El comanament per a generar noves proves és **php artisan make:test NombreDeLaPruebaTest**
En la prova simularem una petició HTTP GET a la URL del mòdul d'usuaris. Amb **assertStatus** comprovem que la URL carrega de forma correcta verificant que el status HTTP siga 200. Amb el mètode **assertSee**  comprovem que podem veure el text “Usuaris”:
 
```php    
    / @test /
    function it_loads_the_users_lists_page(){
    	$this->get(‘usuaris)->assertStatus(200)->assertSee(‘Usuaris’);
    	}
```    

Perquè PHPUnit execute el mètode com una prova, has de col·locar l'anotació / @test / abans de la declaració
del mètode o col·locar el prefix test_ en el nom del mètode com a tal: 


```php   
	function test_it_loads_the_users_lists_page(
		{	
		$this->get(‘usuaris)->assertStatus(200)->assertSee(‘Usuaris’);
		}
```    

En cas contrari el mètode NO serà executat com una prova, la qual cosa és útil perquè ens permet agregar mètodes helpers dins de la nostra classe de proves.

##### Notes

* Pots executar les proves amb vendor/bin/phpunit o crear un àlies per a la consola (àlies t=vendor/bin/phpunit).
* Pots llegir sobre els mètodes d'assercions disponibles en la documentació de Laravel.





