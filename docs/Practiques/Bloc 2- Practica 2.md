## Bloc 2: Laravel


## Pràctica 4

 * Quan un usuari és logueja: 

    * Ha de redirigir a la mateixa pàgina.
    * El seu nom ha d'aparèixer en el botó de logout.
    * Podrà fer like o llevar-lo en els productes (cal crear una ruta per autoritzats, amb el seu mètode al control·lador)
        
```php
// per saber si un usuari ha fet un like sobre un producte
count($this->Likes->where('id',Auth::user()->id))
// per llevar el like o possar en like en un producte
$product->Likes()->detach(Auth::user()->id);
$product->Likes()->attach(Auth::user()->id);
```

## Pràctica 5

Si l'usuari és premium podrà fer un mateniment dels seus productes:

    * Caldrà afegir una camp a l'usuari amb el tipus d'usuari.
    * Hi tindreu que crear un middleware premium.
    * Fer un CRUD amb la taula productes limitat als meus productes

Encara que no és necessari crear un middleware owner perquè als usuaris tan sols
li han d'eixir els propis productes, estaria bé fer-lo per a que un premiun no puga
modificar o esborrar (a cegues) un producte que no és seu. 

```php
public function handle($request, Closure $next,$model)
    {
        $Model = 'App\\Models\\'.$model;
        $id = $request->segments()[1];
        $registre = $Model::findOrFail($id);

        if ($registre->user_id !== Auth::user()->id) {
            abort(403, 'Must Be Owner.');
        }

        return $next($request);
    }
```
i per assignar-lo a una ruta, per exemple:

```php
Route::resource('products', ProductController::class)->except(['index','create','store'])->middleware(['auth','owner:Post']);
```

## Pràctica 6 

Un usuari normal:

    * Podrà llançar ofertes per a un o varios productes.
      * Canvia el camp accepted per a que puga ser nullable i que per defecte o siga. Si és null és que encara el propietari no s'ha decidit. 
      * Afegix un camp sended en la taula ofertes. Esta indicarà si l'oferta ha sigut enviada al propietari del producte.
      * Quan un usuari polse sobre el botó Nova Oferta. Se li obrirà una pantalla amb totes les ofertes que ha fet, la més nova primera.
      * També hi haurà, en la pantalla principal, un enllaç a les meues ofertes, per si l'usuari no vol fer de noves.
      * L'oferta polsada s'haurà afegit amb preu 0 i no enviada. Al acceptar un preu es canviarà l'estat a enviada. 
      * Les ofertes que han sigut rebutjades, podrà tornar a ser enviades amb un preu superior.
      * Podem gastar colors per saver si una oferta ha estat acceptada, rebutjada o encara no s'han decidit.

Un usuari premium:

    * Podrà vore les ofertes rebudes i processar-les.
      * Seria bó no gastar el mateix control·lador per a les ofertes que faig i les que hem fam.
      * Des de la pantala del productes. Polsant sobre un d'ells entre en la pantalla de vore les ofertes sobre eixe produte, adreçades pre preu.
      * Cal un botó per acceptar-les o rebutjar-les.

