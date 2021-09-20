## Bloc 1: PHP UT.2: Introducció

## Pràctica 1

Baixa el [següent repositori](https://classroom.github.com/a/rWnA0DWh) del github classroom, i fes les tasques que s'indiquen a continuació:

1. Separa el fixter index.php creant les vistes que estaran al directori **/views/partials**
	* head.view.php
	* navigation.view.php
	* header.view.php
	* section.view.php
	* footer.view.php

2. La vista section.php mostra els articles. Carrega-los des de l'array **products.php** que està definit dins del fitxer **/config/products.php**
3. La vista **navegation.php** mostra el menu. Carrega-lo des de l'array **menu.php** que està definit dins del fitxer **/config/menu.php**
4. Crea una funció **loadView($vista)** que carregue una vista. Guarda la funció en el fitxer **myHelpers.php**.
5. Modifica el fitxer **index.php** per a que gaste esta funció.