Podstawowe Standardy Programowania
=====================

Ta sekcja standardu zawiera informacje które powinny być rozumiane jako
podstawowe elementy które są wymagane aby zapewnić wysoki poziom 
interoperatywności pomiędzy wpółdzielonym kodem PHP.

Słowa kluczowe "MUSI", "NIE MOŻE", "WYMAGA", "POWINIEN", "NIE POWINIEN", "WINIEN",
"NIE WINIEN", "ZALECANY", "MOŻE", and "OPCJONALNY" użyte w tym dokumencie
mają być interpretowane jak opisano w [RFC 2119].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md


1. Przegląd
-----------

- Pliki MUSZĄ używać tylko tagów `<?php` oraz `<?=`.

- Pliki muszą używać tylko kodowania UTF-8 bez BOM dla kodu PHP.

- Pliki WINNY *albo* deklarować symbole (klasy, funkcje, stałe, etc.)
  *lub* zawierać kod powodujący zmiany (np. generować wyjście, zmieniać ustawienia .ini, etc.)
  ale NIE WINNY robić obu jednocześnie.

- Przestrzenie nazw i klasy MUSZĄ przestrzegać "automatycznego ładowania" wg. PSR: [[PSR-0], [PSR-4]].

- Nazwy klas MUSZĄ być deklarowane z użyciem `StudlyCaps`.

- Stałe klas MUSZĄ być pisane tylko dużymi literami rozdzielone podkreślnikami.

- Nazwy metod MUSZĄ być zadeklarowane z użyciem `camelCase`.


2. Pliki
--------

### 2.1. Znaczniki PHP

Kod PHP MUSI używać długich znaczników `<?php ?>` lub krótkich znaczników "echo" `<?= ?>`; NIE MOŻE używać żadnych innych kombinacji.

### 2.2. Kodowanie znaków

Kod PHP MUSI używać tylko kodowania UTF-8 bez BOM.

### 2.3. Generowanie efektów

Plik POWINIEN deklarować nowe symbole (klasy, funkcje, stałe,
etc.) ale nie generować efektów, lub POWINIEN wykonywać logikę generującą efekty ale
NIE POWINIEN robić obu jednocześnie.

Fraza "generowane efekty" oznacza wykoananie logiki nie związanej bezpośrednio
z deklarowaniem klas, funkcji, stałych etc., *innej niż dołączanie plików*

"Generowane efekty" zawierają ale nie kończą się na: generowaniu wyjścia, jawnym użyciu `require` lub `include`, łączeniu do usług zewnętrznych, modyfikowaniu ustawień ini, generowaniu błędów lub wyjątków, modyfikowaniu globalnych lub statycznych zmiennych, odczytu z lub zapis do pliku, itd.

Poniżej przykład pliku który zawiera zarówno deklaracje jak i generuje efekty;
Przykład kodu którego należy unikać:

```php
<?php
// generowanie efektu: zmiana ustawień ini
ini_set('error_reporting', E_ALL);

// generowanie efektu: wczytanie pliku
include "file.php";

// generowanie efektu: generowanie wyjścia
echo "<html>\n";

// deklaracja
function foo()
{
    // ciało funkcji
}
```

Poniżej przykład pliku który zawiera deklaracje bez generowania efektów;
Przykład kody który należy naśladować:

```php
<?php
// deklaracja
function foo()
{
    // ciało funkcji
}

// deklaracja warunkowa która *nie* jest generowaniem efektu
if (! function_exists('bar')) {
    function bar()
    {
        // ciało funkcji
    }
}
```


3. Przestrzenie nazw i nazwy klas
----------------------------

Przestrzenie nazw i klasy MUSZĄ przestrzegać [PSR-0].

Oznacza to że każda klasa, jest sama w pliku i znajduje się w przestrzeni nazw przynajmniej jednego poziomu: nazwa dostawcy poziomu-głównego

Nazwy klas MUSZĄ być zadeklarowane z użyciem `StudlyCaps`.

Kod pisany dla PHP 5.3 i późniejszych musi używać jawnych przestrzeni nazw.

Na przykład:

```php
<?php
// PHP 5.3 i późniejsze:
namespace Vendor\Model;

class Foo
{
}
```

Kod pisany dla 5.2.x i wcześniejszych POWINIEN używać pseudo przestrzeni nazw pokroju
prefixów `Dostawca_` w nazwach klas.

```php
<?php
// PHP 5.2.x i wcześniejsze:
class Vendor_Model_Foo
{
}
```

4. Stałe klas, atrybuty i metody
-------------------------------------------

Określenie "klasa" odnosi się do wszystkich klas, interfejsów i cech.

### 4.1. Stałe

Stałe klas MUSZĄ muszą być zadeklarowane z użyciem wielkich liter oddzielone podkreślnikami.
Na przykład:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Atrybuty

Ten przewodnik celowo unika jakichkolwiek rekomendacji odnośnie
użycia standardów `$StudlyCaps`, `$camelCase` lub `$under_score`
dla nazw atrybutów.

Jakkolwiek przyjęta konwencja POWINNA być stosowana konsystentnie w sensownym zakresie.
Tym zakresem może być poziom-dostawcy, poziom-pakietu, poziom-klasy lub poziom-metody.

### 4.3. Metody

Nazwy metod MUSZĄ być deklarowane z użyciem `camelCase()`.
