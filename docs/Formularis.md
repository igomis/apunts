Formularis

## Pas de paràmetres

Una **petició http** és la sol·licitud d'un recurs al servidor:
  * Es realitzen a través d'una url
  * Es poden passar paràmetres amb la petició
* Hi ha diferents mètodes (**METHOD**) de realitzar una petició **(GET, POST, PUT, DELETE, PATCH, etc.)**, encara que els més habituals són GET i POST

### Petició GET
* S'utilitza per a sol·licitar dades d'un recurs
  * Es Mostren els paràmetres que s'envien en la url
  * Es poden utilitzar directament en enllaços
  * Romanen en l'historial del navegador
  * La grandària dels paràmetres està limitat a 255 caràcters

![](../img/t207.png)

![](../img/t208.png)

##### Accedir a les dades de la petició GET

* Usem la variable superglobal **$_GET**
  * És un array associatiu
  * Les claus del array coincidiran amb els noms que li hem donat als paràmetres.
  * Per a accedir als paràmetres de la petició anterior:

```php
echo $_GET['nom'] . ' ' . $_GET['cognom'];
```
##### Evitar el CSRF

En tota pàgina que reba paràmetres GET has de comprovar el **HTTP referer** del navegador, i que aquest siga de dins de la teua web. En php el referer que envia el navegador s'emmagatzema en $_*SERVER['HTTP_REFERER']

Seria tal com:

 ```php
 if( parse_url($_SERVER['HTTP_REFERER'], PHP_URL_HOST) != $_SERVER['HTTP_HOST']) die('Anti-CSRF'); 
 ```

NOTA IMPORTANT
Amb aquest codi estem obligant al fet que el navegador envie un referer si o sí. Per tant només ha d'utilitzar-se en pàgines a les quals el navegador accedisca des d'una altra pàgina de la nostra web.
Òbviament no podem col·locar-ho en la primera pàgina a la qual s'accedeix a la nostra web (index.php o similar), ja que si l'usuari a escrit l'adreça a mà en la barra del navegador no s'enviarà referer cap i saltarà el sistema.

### Petició POST
* S'utilitza per a enviar dades a un recurs
  * Els paràmetres van en el cos de la petició, no són visibles per a l'usuari
  * No es pot utilitzar en un enllaç
  * No roman en l'historial
  * Se solen utilitzar en els formularis
  * No tenim la limitació de grandària dels paràmetres

![](../img/t209.png)

![](../img/t210.png)


##### Accedir a les dades de la petició POST
* Usem la variable superglobal **$_POST**
  * Funciona igual que $_GET.
  * Mostrar totes les dades rebudes:
 
```php
var_dump($_POST);
```
	
  * Mostrar les dades individualment:
   
```php
echo $_POST['nom']; echo $_POST['cognom'];
```

## Formularis

Al crear un formulari li assignem un metode (habitualment POST) i una destinació (pàgina que tractarà el formulari)

```php
<form method='POST' action='pagina.php'>
```


#### Enviar les dades al mateix script que mostra el formulari

És habitual que la pàgina que tracta el formulari siga la mateixa que la que el conté. Per poder fer-ho: 

* La variable **$_SERVER** conté dades relacionades amb l'entorn del servidor de la petició HTTP
  * Una de les dades que conté és el script php que s'està executant **($_SERVER['PHP_SELF'])**. Si indiquem el action del formulari així: **action="\<?= $_SERVER['PHP_SELF']; ?>"**, serà la pròpia pàgina del formulari la que processe les dades.
  
* Si és així haurem de programar dos blocs:
	* Un inicial de comprovació que s'executa quan s'accedix pel mètode POST,es a dir, a l'enviar el formulari. Si les dades són correctes es processarà el formulari. En cas contrari es crea la variable **$err** i continua l'script.
  	* L'HTML, que s'executa quan s'accedix mitjançant GET (no ve del formulari) o després d'executar el primer bloc, si així ho volem.

##### Verificar que el formulari s'ha enviat
* Abans de mostrar les dades verificarem que s'haja enviat el formulari:


```php
<?php
/* si va bien redirige a principal.php si va mal, mensaje de error */
	if ($_SERVER["REQUEST_METHOD"] == "POST") {  
		if($_POST['usuario'] === "usuario" and $_POST["clave"] === "1234"){		header("Location: principal.php");
		}else{
			$err = true;
		}	
	}
?>
<!DOCTYPE html>
<html>
	<head>
		<title>Formulario de login</title>		
		<meta charset = "UTF-8">
	</head>
	<body>			
		<?php if(isset($err)){
			echo "<p> Revise usuario y contraseña</p>";
		}?>
		<form action = "<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>" method = "POST">
			<label for = "usuario">Usuario</label> 
			<input value = "<?php if(isset($usuario))echo $usuario;?>"
			id = "usuario" name = "usuario" type = "text">				
			
			<label for = "clave">Clave</label> 
			<input id = "clave" name = "clave" type = "password">			
			
			<input type = "submit">
		</form>
	</body>
</html>
```
     
##### Accedir a paràmetres no existents

![](../img/t211.png)

#### Validació del formulari
* Hem de comprovar que les dades del formulari són correctes
* Validacions a realitzar:
  * Els camps requerits no han de quedar buits 
  * Els camps email i data han de tenir el format esperat 
  * Tots els camps s'han de filtrar.
 
###### Valors buits
* Els camps requerits no haurien de quedar-se buits.
* Per a verificar que un valor no queda buit podem utilitzar la funció **empty()** de PHP. <http://php.net/manual/es/function.empty.php> .

###### Espais en blanc
* Hem d'eliminar els espais en blanc del principi i final dels camps
* S'utilitza la funció **trim()**

###### Filtrar l'entrada
* Sempre hem de filtrar l'entrada amb **htmlspecialchars()** abans de mostrar el camp amb echo o similar. Si mostrem l'entrada tal cual podem patir atacs XSS o d'injecció.



###### Comprovar l'email
* Per a verificar si un email és correcte podem utilitzar la funció **filter_var**($email, FILTER_VALIDATE_EMAIL)) <http://php.net/manual/es/function.filter-var.php>

###### Dates
* En PHP les dates s'emmagatzemen com a números.
* S'utilitza la classe **DateTime** per a representar-les
* Igual que ocorre amb les cadenes no té sentit estudiar les funcions relacionades una a una. Les pots consultar en: <http://php.net/manual/es/ref.datetime.php>
* Per a comprovar la data devem crear una funció a aquest efecte
* Podemos utilitzar el mètode **createFromFormat()** de la classe DateTime <http://php.net/manual/es/datetime.createfromformat.php>

#### Pujada de fitxers

Es un cas especial. En primer lloc en el formulari cal utilitzar l'atribut enctype="multipart/form-data" i el metode POST. Per al fitxer s'utilitza una etiqueta \<input type="file">.

 ```php
<!DOCTYPE html>
<html>
	<body>
		<form action="procesar_subida.php" method="post" enctype="multipart/form-data">    
			Escoja un fichero
			<input type="file" name="fichero">
			<input type="submit" value="Subir fichero">
		</form>
	</body>
</html>
 ```
 
En l'script que reb el fitxer la variable global *$_FILES* conté la informació sobre el fitxer en un array bidimensional.

#### Elements del array del fitxer pujat
* **$_FILES['imatge']['tmp_name']**: lloc i nom de l'arxiu temporal en el servidor.
* **$_FILES['imatge']['name']**: Nom original del fitxer en la màquina client.
* **$_FILES['imatge'][size']**: Grandària en bytes del fitxer pujat.
* **$_FILES['imatge'][type']**: Tipus MIME associat al fitxer. Per exemple, "image/gif" o "text/plain".
* **$_FILES['imatge'][error']**: Codi d'error associat al fitxer pujat.  

```php
<?php
	$tam = $_FILES["fichero"]["size"];
	if($tam > 256 *1024){
		echo "<br>Demasiado grande";
		return;
	}
	echo "Nombre del fichero: " . $_FILES["fichero"]["name"];
	echo "<br>Nombre temporal del fichero en sel servidor: " . $_FILES["fichero"]["tmp_name"];	
	$res = move_uploaded_file($_FILES["fichero"]["tmp_name"],"subidos/" . $_FILES["fichero"]["name"]);
    if($res){
		echo "<br>Fichero guardado";
    } else {
        echo "<br>Error";
    }
 ```
 
#### Grandària del fitxer a pujar
* Podemos fixar el límit de grandària en el fitxer php.ini
	* http://php.net/manual/es/ini.core.php#ini.upload-max-filesize
* També podem fixar el límit en el propi formulari
* A través d'un camp ocult (type="hidden") denominat **MAX_FILE_SIZE**.

 ```php
<input type="hidden" name="MAX_FILE_SIZE" value="1000000">
 ```

#### Ruta temporal del fitxer pujat
* En carregar un arxiu, es guardarà en una ubicació temporal indicada per l'opció upload_tmp_dir en el php.ini.
	* http://php.net/manual/es/ini.core.php#ini.upload-tmp-dir
* Si no movem l'arxiu o ho canviem de nom, quan acabe l'execució del script, este serà eliminat.


#### Problemes més habituals
* Especificar en upload\_tmp\_dir un directori al com no es té accés
* La directiva memory\_limit té un valor molt baix o inferior a upload_max_filesize
* La directiva max\_execution\_time té un valor baix i el script ho excedeix durant la pujada del fitxer
* La directiva post\_max\_size té un valor baix i el teu fitxer ho excedeix. El seu valor ha de ser major a upload\_max\_filesize

##### Codis d'error
* El primer que caldrà fer és comprovar el codi d'error del fitxer pujat **($_FILES['imatge'][error'])**
* Si és diferent de **UPLOAD_ERR_OK** hi ha hagut algun problema
* Veure els codis d'error:
	* http://php.net/manual/es/features.file-upload.errors.php

##### Comprovació del tipus de fitxer
* A continuació comprovarem que el tipus myme està dins dels esperats:

```php
if ($_FILES['imatge']['type'] !== 'image/gif') {
	echo 'Error: No es tracta d'un fitxer .GIF.'; 
	exit; 
 }
```

##### Evitar atacs
* Ataque comú:
	* http://seclists.org/bugtraq/2000/sep/55
* Per a evitar aquest tipus d'atacs es va incloure la funció:
	* bool is\_uploaded\_file ( string nom_arxiu )
* Retorna true si l'arxiu donat va ser carregat a través d'HTTP POST
* Li passarem el paràmetre **$_FILES['imatge']['tmp_name']**

##### Moure l'arxiu pujat
* Per a moure l'arxiu temporal a la seua ubicació correcta usem la funció
	* bool move\_uploaded\_file(string nom\_arxiu, string destinació)
* Aquesta funció s'assegura que l'arxiu siga un arxiu carregat vàlid.
* Si existeix un arxiu destine amb el mateix nom, est serà sobreescrit.

##### Evitar sobrescritures
* Abans de moure l'arxiu pujat és convenient comprovar que no existeix un arxiu amb el mateix nom
	* bool is\_file(string nom\_arxiu) 
	* Li passarem $_FILES['imatge']['name']
* En cas que existisca caldrà canviar el nom, per exemple afegint una marca de temps com a prefix

És el moment de fer més exercisis: [Exercisi 2.4 Formularis](2.4.Activitat.md)