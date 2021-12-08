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

* Crea el models Product,Category, ja que User ja està creada
* Crea les relacions entre els models també ací.
* Like i Offer no són en si un model sinó una relació molts a molts entre Product i User.

## Pràctica 3

* Fes que a la pàgina principal es mostres tots el productes pàginats de 8 en 8.
* Fes que funcionen els menus de mostrar els més nous i els més valorats. 