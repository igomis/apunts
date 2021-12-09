## Bloc 2: Laravel

## Pràctica 1

Crea les migracions per al BatoiPOP:

**Products (id, name, original_price, discount_price, sale, category_id, img, user_id)** amb els timestamps

**Categories (id,name,img)** sense timestaps

**Offers (id,product_id,user_id,price,accepted)** amb els timestamps

**Likes (product_id, user_id)** amb els timestamps

S'han de crear també les relacions entre les tables.

Crea els seeders i el factories necessaris per donar d'alta

* 500 usuaris
* 250 productes d'usuaris aleatoris i de 10 categories
* 100 ofertes. Has de tindre en compte que si hi ha oferta el camp sale del producte estarà a **true**.
* 4000 Likes de productes aleatoris 

## Pràctica 2

* Crea el models Product,Category,Offer ja que User ja està creada
* Crea les relacions entre els models també ací.
* Like no és en si un model sinó una relació molts a molts entre Product i User.

## Pràctica 3

* Fes que a la pàgina principal es mostres tots el productes pàginats de 8 en 8.
* Fes que funcionen els menus de mostrar els més nous i els més valorats. 

## Coses a tindre en compte

* Els numeros en els seeder son aproximats, no cal que siguen exactes
* Un usuari aleatori pot ser un numero aleatori de 1 a 500.
* Dins dels mètodes dels factories es poden utilitzar variables per fer calculs.
* Per a crear una relació likes entre usuaris i productes es pot utilitzar el mètode [attach](https://laravel.com/docs/8.x/eloquent-relationships#attaching-detaching)
* Si poseu una imatge per defecte en cada producte, s'estalvieu un @if en la vista.
* La vista products/fitxa està per adaptar
* Podeu gastar un controlador LandingPage per a gestionar les pàgines d'inici no loguejades ja que el HomeController necessita Autorització.
* Haureu de modificar el menu que està en config/menu.php per canviar els enllaços per adaptar-los al PSR-4.
  Així tindrem /populars /news en compte de /?opcion=populars.
* Cal mirar com [paginar en bootstrap](https://laravel.com/docs/8.x/pagination#using-bootstrap)
* Per a saber com ordenar per numero de likes podeu mirar el següent [enllaç](https://stackoverflow.com/questions/24208502/laravel-orderby-relationship-count)