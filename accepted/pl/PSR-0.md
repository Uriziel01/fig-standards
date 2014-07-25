Standard automatycznego modułu ładującego
====================

Poniżej opisane zostały wymagania które muszą być przestrzegane
by zapewnić interoperacyjność automatycznego modułu ładującego.

Wymagania
---------

* Pełna przestrzeń nazw oraz klasa muszą mieć następującą strukturę
 `\<Nazwa dostawcy>\(<Przestrzeń nazw>\)*<Nazwa klasy>`
* Każda przestrzeń nazw musi mieć główną sekcję równą ("Nazwa dostawcy").
* Każda przestrzeń nazw może posiadać tyle podprzestrzeni ile tylko chce.
* Każdy separator sekcji w przestrzeni nazw jest zamieniany na `DIRECTORY_SEPARATOR` podczas ładowania pliku z systemu plików.
* Każdy znak `_` w NAZWIE KLASY jest zamieniany na
  `DIRECTORY_SEPARATOR`. Znak `_` nie ma specjalnych zastosowań w przestrzeni nazw.
* Pełna przestrzeń nazw jest afiksowana ciągiem `.php` podczas ładowania z systemu plików.
* Litery wewnątrz nazwy dostawcy, przestrzeni nazw i klasy mogą być dowolną kombinacją wielkich i małych znaków.

Przykłady
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/sciezka/do/projektu/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/sciezka/do/projektu/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/sciezka/do/projektu/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/sciezka/do/projektu/lib/vendor/Zend/Mail/Message.php`

Podkreślnik w Przestrzeniach Nazw i Nazwach Klas
-----------------------------------------

* `\namespace\package\Class_Name` => `/sciezka/do/projektu/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/sciezka/do/projektu/lib/vendor/namespace/package_name/Class/Name.php`

Ustanowiony tak standard powinien być najmniejszym wspólnym mianownikiem
dla zapewnienia bezbolesnej interoperacyjności modułu łądującego.
Możesz sprawdzić czy jesteś zgodny z tymi standardami wykorzystując
poniższą przykłądową implementację SplClassLoader która jest
zdolna do ładowania klass z PHP 5.3.

Przykładowa implementacja
----------------------

Poniżej zaprezentowany jest przykład jak wygląda proces automatycznego
ładowania według zaproponowanych powyżej standardów.

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader Implementation
-----------------------------

The following gist is a sample SplClassLoader implementation that can
load your classes if you follow the autoloader interoperability
standards proposed above. It is the current recommended way to load PHP
5.3 classes that follow these standards.

* [http://gist.github.com/221634](http://gist.github.com/221634)

