#### Configuració projecte

Per a configurar un projecte amb composer i phpunit per a l'entorn de proves, anem a utilitzar el següent fitxer composer.json

	{
	    "autoload":{
	        "psr-4":{
	            "Ejercicios\\": "src/"
	        },
	        "files": [
	            "bootstrap/helpers.php",
	            "bootstrap/errorsReport.php"
	        ]
	    },
	    "require-dev": {
	        "phpunit/phpunit": "^8.0",
	        "mockery/mockery": "^1.2"
	    },
	
	    "autoload-dev": {
	        "psr-4":{
	            "Tests\\" : "tests/"
	        }
	    },
	    "require": {
	        "filp/whoops": "^2.3",
	        "firephp/firephp-core": "dev-master"
	    }
	}
	
i fem un **composer update**. 

Este fitxer instal.la el phpunit i mockery per a proves, configura el directori base per el namespace en el directori src Instal.lem el firephp per a poder vore en la consola del navegador el missatges que li enviem des del servidor (també haurem d'instal.lar l'extensió en el nostre navegador)

Les classes les declararem en el directori /src. Tindrem un directori bootsrap on posarem el fitxer helpers.php (de moment buid) i que es carregarà abans del projecte on podem ficar funcions compartides per tots el fitxers. També carreguem un entorn per a visualitzar els errors de manera gràfica en bootstrap/errorsReport.php.
El nostre projecte començarà carregant el fitxer index.php el posarem en el directori **public**.

### Clase Formulari

La idea es crear classes per a poder pintar un formulari.