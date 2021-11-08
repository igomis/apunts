## Proves

Per a executa proves la ferramenta més utilitzada en php és phpunit.

```
composer require --dev phpunit/phpunit
```

L'element base del phpunit es el **cas de prova**, una classe que ha d'heretar de **TestCase** de la llibreria. Dins d'un cas de prova hi ha varias proves.
Per a provar una classe de nom **Usuari** es crearà una classe de nom **UsuariTest**. Esta classe contindrà una sèrie de mètodes amb les proves que es volen realitzar. Estos mètodes han de ser publics, encara que les classe podrà contindre altres mètodes auxiliars per tal de realitzar les proves.
Per a indicar que el mètode és una prova hi ha dues opcions:

* Que el nombre començe per test

```php
public function testCanCreateUsuari()
```

* Utilitzar un bloc de comentaris específics amb la notació test  

```php
/**
/* #test
*/
public function testCanCreateUsuari()
```

Dins d'estos mètodes, la majoria de les comprovacions es realitzen utilitzant **asserts**, mètodes definits en la classe **TestCase**, que comprova si es complixen o no algunes condicions. Si alguna de les assercions no s'acomplixen es considera que la prova ha fallat.

Per exemple podem provar la classe següent:

```php
<?php
   class Matematicas{
	/* funcion factorial */
		public static function factorialEx($num){
			if ($num < 0) {
				throw new InvalidArgumentException("N�mero negativo");
			}	
			$resul = 1;
			for($i=2; $i <= $num; $i++){
				$resul = $resul * $i;
			}
			return $resul;
		}
   }
```
  
amb la següent classe:

```php
<?php
	require "vendor/autoload.php";
	require "Matematicas.php";
	use PHPUnit\Framework\TestCase;
	class MatematicasTest extends TestCase{    		
		public  function testCero(){
			$this->assertEquals(1, Matematicas::factorialEx(0));						
		}		
		/**
		* @test
		* @expectedException InvalidArgumentException
		*/
		public static function Excepcion(){
			//equivalente a la anotación  @expectedException
			//$this->expectException(InvalidArgumentException::class);
			return Matematicas::factorialEx(-1);			
			
		}
	}   
```

on el primer mètode comprova que calcula be el factorial de 0, i el segon mètode comprova que si el paràmetre és negatiu es llança una exempció.

Per a executar la prova cal executar phpunit des de la línea de comanaments

```
 /vendor/bin/phpunit MatematicasTest.php
```

Més exemples: Podem provar les funcions que ja hem fet i que tenim al myHelpers:

```php
<?php
/**
 * Created by PhpStorm.
 * User: igomis
 * Date: 2019-10-17
 * Time: 22:59
 */

require dirname(__FILE__) . "/../vendor/autoload.php";
$whoops = new \Whoops\Run;
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
$whoops->register();

use PHPUnit\Framework\TestCase;

class FunctionsTest extends TestCase
{
    public function test_calculadora()
    {
        $this->assertEquals(13,suma(6,7));
        $this->assertEquals(-1,resta(6,7));
        $this->assertEquals(42,multiplicacion(6,7));
        $this->assertEquals('4 + 6 = 10',calculadora('+',4,6));
    }
}
```

o les classes Form i SIInput per a generar formulari

```php
<?php
/**
 * Created by PhpStorm.
 * User: igomis
 * Date: 2019-10-17
 * Time: 22:59
 */

require dirname(__FILE__) . "/../vendor/autoload.php";
$whoops = new \Whoops\Run;
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
$whoops->register();

use Ejercicios\Form;
use Ejercicios\SIInput;
use PHPUnit\Framework\TestCase;


class FormTest extends TestCase
{

    public function test__construct()
    {
        $form = new Form([new SIInput('Usuari','usuario'),new SIInput('Password','password','password')]);
        $this->assertIsArray($form->getComponents());
        $this->assertCount(2,$form->getComponents());
    }

    public function testRender()
    {
        $form = new Form([new SIInput('Usuari','usuario'),new SIInput('Password','password','password')]);
        $this->assertStringContainsString('password',$form->render());
    }
}
```

```php
<?php
/**
 * Created by PhpStorm.
 * User: igomis
 * Date: 2019-10-17
 * Time: 22:59
 */

require dirname(__FILE__) . "/../vendor/autoload.php";
$whoops = new \Whoops\Run;
$whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler);
$whoops->register();

use Ejercicios\SIInput;
use PHPUnit\Framework\TestCase;



class SIInputTest extends TestCase
{
    public function test_works_textArea()
    {
        $input = new SIInput('Usuari','usuario','textarea');
        $this->assertStringContainsString('textarea',$input->render());
    }
    public function test_works_textInput()
    {
        $input = new SIInput('Usuari','usuario');
        $this->assertStringContainsString('text',$input->render());
    }

    public function test_works_textPass()
    {
        $input = new SIInput('Usuari','usuario','password');
        $this->assertStringContainsString('password',$input->render());
    }
}

```
Com veiem cada test conté una instaciació de l'element que es vol provar i una comprobaciò del resultat.

Per a més informacio [phpunit](https://phpunit.readthedocs.io/es/latest/writing-tests-for-phpunit.html)
