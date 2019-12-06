########################
Wstępne parsowanie pliku
########################

Część konfiguracji przed właściwym użyciem przez silnik jest wstępnie parsowana.
Ten proces może być kontrolowany przy użyciu dyrektyw preprocesora.

Dyrektywy parsowania
********************

Dyrektywy warunkowe - @anyOf i @oneOf
=====================================

Dopuszczalne sa następujące formaty dyrektywy:

* Pełna

.. code-block:: json

    {
      "@anyOf|@oneOf": {
        "items": [
            {
              "if": "reguła",
              "content": "zawartość do wstawienia przy spełnieniu warunku"
            }
        ],
        "options": {
        }
      }
    }

* Skrócona definicja

.. code-block:: json

    {
      "@anyOf|@oneOf": [
        {
          "if": "reguła",
          "content": "zawartość do wstawienia przy spełnieniu warunku"
        }
      ],
    }

W skróconej wersji nie jest możliwe przekazanie dodatkowych opcji.

@anyOf
------

Dyrektywa pozwala na zdefiniowane listy konfiguracji które mają być wybrane jeśli zostaną spełnione określone warunki.

Dodatkowe opcje:

+----------+------------------------------------------+----------+-----------------+
| Parametr |                   Opis                   | Wymagane |      Uwagi      |
+==========+==========================================+==========+=================+
| merge    | Opcja określa czy przy kilku spełnionych | NIE      | Domyślnie false |
|          | warunkach ich zawartość ma był połączona |          |                 |
+----------+------------------------------------------+----------+-----------------+

@oneOf
------

Dyrektywa pozwala na wybranie wyłącznie jednego wariantu po spełnieniu warunku.
Przy parsowaniu jeden warunek **MUSI** zostać spełniony, więc zalecane jest,
aby ostatni warunek był określony przez wyrażenie true. Jeśli warunek zostanie spełniony parsowanie zostanie zatrzymane.


Poniższa tabela przedstawia najważniejsze różnice miedzy dyrektywami @oneOf i @anyOf:

+--------------------------------------------------+---------------------------------------------+
| @oneOf                                           | @anyOf                                      |
+==================================================+=============================================+
| Przynajmniej jeden warunek musi zostać spełniony | Żaden warunek nie musi być spełniony        |
+-------------------------------------+------------+---------------------------------------------+
| Maksymalnie jeden warunek może zostać spełniony  | Wszystkie warunki mogą być spełnione        |
+--------------------------------------------------+---------------------------------------------+
| Parsowanie zostanie zatrzymane przy|             | Parsowanie zostanie zatrzymane              |
| pierwszym spełnionym warunku                     | po sprawdzeniu wszystkich warunków          |
+--------------------------------------------------+---------------------------------------------+


Jeśli reguła jest skomplikowana i składa sie z kilku warunków możliwy jest podział reguły na listę warunków:

.. code-block:: json

    {
      "@anyOf|@oneOf": [
        {
          "if": [
            {
              "rule": "reguła1"
            },
            {
              "rule": "reguła2"
              "type": "OR/AND"
            }
          ],
          "content": "zawartość do wstawienia przy spełnieniu warunku"
        }
      ],
    }

Pętla - @repeat
===============

Dyrektywa pozwala na cykliczne powielenie konfiguracji, przechodząc po kolekcji znajdującej się w kontekście silnika.

.. code-block:: json

  {
    "@repeat": {
      "var": "nazwa zmiennej pod którą biędzie dostępny obecny element",
      "in": "kolekcja po której następuje iteracja",
      "content": "zawartość do powielenia"
    }
  }

Przykład:

.. code-block:: json

  {
    "@repeat": {
      "var": "i",
      "in": "testArray",
      "content": "{{i.id}}"
    }
  }

Wyrażenia - expression
======================

Dyrektywa pozwala na wstawienie wyniku wyrażenia lub zmiennych we wskazane miejsce.
Wyrażenie **MUSI** być wstawione w postaci **{{wyrażenie}}**.
W jednym polu może znajdować się wiele bloków wyrażeń.

Przykład:

.. code-block:: json

    {
        "exp1" : "{{settings[0]}} {{settings[1]}}",
        "exp2" : "{{settings[0] * settings[1]}}"
    }

Jeśli wynikiem wyrażenia jest liczba lub typ logiczny i jest to jedyne wyrażenie w danym polu to wynik jest automatycznie przekonwertowany.

Przykład:

.. code-block:: json

    {
        "exp1" : "{{1}}",
    }

Wynikiem będzie:

.. code-block:: json

    {
        "exp1" : 1,
    }

Formatowanie - @formatter
=========================

Dyrektywa pozwala na przeformatowanie bloku konfiguracji z jednego typu na drugi.

.. code-block:: json

    {
      "type": "typ konwertera",
      "config": {
      },
      "value": {}
    }


string
------

Konwersja do łańcucha znaków.
Używane głównie przy dyrektywie **@anyOf** do generowania tekstu zależnie od warunków.

Opcje - config:

+----------+-----------------------------+----------+-------+
| Parametr |            Opis             | Wymagane | Uwagi |
+==========+=============================+==========+=======+
| concat   | Łącznik fragmentów łańcucha | NIE      |       |
+----------+-----------------------------+----------+-------+

.. code-block:: json

    {
      "@formatter": {
        "type": "string",
        "config": {
          "concat": " and "
        },
        "value": {
          "@anyOf": [
            {
              "if": true,
              "content": "Hello"
            },
            {
              "if": true,
              "content": "World"
            }
          ]
        }
      }
    }

Czego wynikiem będzie "Hello and World".

Przetwarzanie szablonu TWIG - @twigRender
=========================================

Dyrektywa pozwala na wstawienie wyniku parsowania zewnętrznego pliku Twig_. Kontekstem parsowania pliku jest kontekstem preparsera.

.. _Twig: https://twig.symfony.com/

.. code-block:: json

    {
      "@twigRender": {
        "file": "ścieżka do pliku relatywnie do system.json"
      },
      "options": {
      }
    }

Opcje - options:

+----------+-----------------------------------------------------------------+----------+---------------------------------------------------------------------------+
| Parametr |                              Opis                               | Wymagane |                                   Uwagi                                   |
+==========+=================================================================+==========+===========================================================================+
| wrap     | Pozwala na wstawienie tekstu przed i po wyniku parsowania twiga | NIE      | Aby wstawić treść wygenerowana z szablonu twiga należy użyć **{content}** |
+----------+-----------------------------------------------------------------+----------+---------------------------------------------------------------------------+

Przykład:

.. code-block:: json

    {
      "@twigRender": {
        "file": "twig/test.twig"
      },
      "options": {
        "wrap": "<!- {content} -->"
      }
    }


Kontekst parsowania
*******************

Kontekst jest zbiorem zmiennych oraz funkcji które można użyć w powyższych dyrektywach.
Dostępność niektórych zmiennych zależy od parsowanego fragmentu konfiguracji.

Funkcje pomocnicze - utils
==========================

Funkcje dostępne są w każdym parsowanym fragmencie

isEmpty
-------

Sprawdzenie czy zmienna posiada wartość. Odpowiednik empty_ z języka PHP.

Zwracana wartość: Boolean 

Parametry:

+----------+------------------------+----------+-------+
| Parametr |          Opis          | Wymagane | Uwagi |
+==========+========================+==========+=======+
| data     | Zmienna do sprawdzenia | TAK      |       |
+----------+------------------------+----------+-------+

.. _empty: https://www.php.net/manual/en/function.empty.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.isEmpty(data)}}"
    }

implode
-------

Łączenie elementów w łańcuch znaków. Odpowiednik implode_ z języka PHP.

Zwracana wartość: String 

Parametry:

+----------+--------------------------------+----------+-------+
| Parametr |              Opis              | Wymagane | Uwagi |
+==========+================================+==========+=======+
| data     | Tablica elementów do złączenia | TAK      |       |
+----------+--------------------------------+----------+-------+
| glue     | Łącznik elementów              | TAK      |       |
+----------+--------------------------------+----------+-------+

.. _implode: https://www.php.net/manual/en/function.implode.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.implode(', ', data)}}"
    }

round
-----

Zaokrąglenie liczby zmiennoprzecinkowej. Odpowiednik round_ z języka PHP.

Zwracana wartość: Float 

Parametry:

+-----------+------------------------------------+----------+-------------+
| Parametr  |                Opis                | Wymagane |    Uwagi    |
+===========+====================================+==========+=============+
| val       | Liczba do Zaokrąglenia             | TAK      |             |
+-----------+------------------------------------+----------+-------------+
| precision | Precyzja(liczba cyfr po przecinku) | NIE      | Domyślnie 0 |
+-----------+------------------------------------+----------+-------------+

.. _round: https://www.php.net/manual/en/function.round.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.round(price, 2)}}"
    }

escape
------

Dekodowanie łańcucha znaków do postaci bezpiecznej do użycia w Twig

Zwracana wartość: String 

Parametry:

+----------+-----------------------+----------+-------+
| Parametr |         Opis          | Wymagane | Uwagi |
+==========+=======================+==========+=======+
| str      | Tekst do zdekodowania | TAK      |       |
+----------+-----------------------+----------+-------+

createDateTime
--------------

Tworzenie obiektu DateTime_.

Zwracana wartość: DateTime_

.. _DateTime: https://www.php.net/manual/en/class.datetime.php

Parametry:

+----------+-----------------------------------------------------+----------+--------------------------------------+
| Parametr |                        Opis                         | Wymagane |                Uwagi                 |
+==========+=====================================================+==========+======================================+
| time     | formuła dnia i godziny                              | NIE      | Domyślnie "now". Zgodnie z formatem_ |
+----------+-----------------------------------------------------+----------+--------------------------------------+
| timezone | Formuła strefy czasowej lub przesunięcie(np. +0200) | NIE      | Domyślnie null. Zgodnie z strefami_  |
+----------+-----------------------------------------------------+----------+--------------------------------------+


.. _formatem: https://www.php.net/manual/en/datetime.formats.php
.. _strefami: https://www.php.net/manual/en/timezones.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.createDateTime('2019-12-06 12:00:00').getTimeStamp()}}"
    }

includes
--------

Sprawdzenie czy jakiś tekst występuje w innym. Odpowiednik strpos_ z języka PHP.

Zwracana wartość: Boolean 

Parametry:

+-----------+---------------------------------------+----------+-------+
| Parametr  |                 Opis                  | Wymagane | Uwagi |
+===========+=======================================+==========+=======+
| str       | Tekst w którym następuje wyszukiwanie | TAK      |       |
+-----------+---------------------------------------+----------+-------+
| substring | Tekst do wyszukania                   | TAK      |       |
+-----------+---------------------------------------+----------+-------+

.. _strpos: https://www.php.net/manual/en/function.strpos.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.includes('Hello World', 'Hello')}}"
    }

replace
-------

Zamiana tekstu na inny. Odpowiednik str_replace_ z języka PHP.

Zwracana wartość: String 

Parametry:

+----------+---------------------------------------------------+----------+-------+
| Parametr |                       Opis                        | Wymagane | Uwagi |
+==========+===================================================+==========+=======+
| search   | Tekst lub tablica do wyszukania                   | TAK      |       |
+----------+---------------------------------------------------+----------+-------+
| replace  | Tekst lub tablica na które ma być dokonana zmiana | TAK      |       |
+----------+---------------------------------------------------+----------+-------+
| subject  | Tekst w którum następuje wyszukiwanie             | TAK      |       |
+----------+---------------------------------------------------+----------+-------+

.. _str_replace: https://www.php.net/manual/en/function.str-replace.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.includes('World', 'Universe', 'Hello World')}}"
    }

arrayFilter
-----------

Filtrowanie elementów po przekazanych parametrach

Zwracana wartość: Array 

Parametry:

+----------+------------------------------------------------+----------+-------+
| Parametr |                      Opis                      | Wymagane | Uwagi |
+==========+================================================+==========+=======+
| input    | Tablica wejściowa                              | TAK      |       |
+----------+------------------------------------------------+----------+-------+
| params   | Obiekt parametrów filtra lub JSON tego obiektu | TAK      |       |
+----------+------------------------------------------------+----------+-------+

Przykład:

.. code-block:: json

    {
      "test": "{{utils.arrayFilter(data, '{\"param\": \"val\"}'}}"
    }

arrayColumn
-----------

Zwraca tablicę z wartościami z wskazanej kolumny. Odpowiednik array_column_ z PHP.

Zwracana wartość: Array 

Parametry:

+----------+-------------------------------------+----------+-------+
| Parametr |                Opis                 | Wymagane | Uwagi |
+==========+=====================================+==========+=======+
| input    | Tablica wejściowa                   | TAK      |       |
+----------+-------------------------------------+----------+-------+
| param    | Klucz/kolumna która ma być zwrócona | TAK      |       |
+----------+-------------------------------------+----------+-------+

.. _array_column: https://www.php.net/manual/en/function.array-column.php

Przykład:

.. code-block:: json

    {
      "test": "{{utils.arrayColumn(data, 'param')}}"
    }


Parametry konta - session
=========================

getHubId()
----------

Pobranie identyfikatora konta

Przykład:

.. code-block:: json

    {
      "test": "{{session.getHubId()}}"
    }

Treści - contentRepository
==========================

Pobranie treści z systemu.

.. code-block:: json

    {
      "test": "{{contentRepository.getContent('typTreści', identyfikator)}}"
    }

Dostępne treści

* personalization - szablon personalizacji
* webPushTemplate - szablon powiadomienia push
* staticContent - treść statyczna

Przykład:

.. code-block:: json

    {
      "test": "{{contentRepository.getContent('webPushTemplate', 1).getTitle()}}"
    }


mediaHelper
===========

generatePublicUrl
-----------------

Pozwala na wygenerowanie publicznego url do określonego zasobu

Parametry:

+----------+--------------------------------+----------+---------------------+
| Parametr |              Opis              | Wymagane |        Uwagi        |
+==========+================================+==========+=====================+
| hubId    | Identyfikator konta w systemie | TAK      |                     |
+----------+--------------------------------+----------+---------------------+
| media    | Obiekt zasobu                  | TAK      |                     |
+----------+--------------------------------+----------+---------------------+
| type     | Typ zasobu                     | NIE      | Domyślnie reference |
+----------+--------------------------------+----------+---------------------+
| absolute | Czy ścieżka ma być pełna       | NIE      | Domyślnie: true     |
+----------+--------------------------------+----------+---------------------+

Przykład:

.. code-block:: json

    {
      "test": "{{mediaHelper.generatePublicUrl(session.hubId, notification.icon}"
    }

Ustawienia bloczka
==================

Podczas zapisu bloczka (sekcja :doc:`save </save>`) w kontekscie zapisane są wszystkie uzupełnione wartości z formularza. 
Parametry dostępne są w obiekcie **settings**.

.. code-block:: json

    {
      "settings": {
        "param1": "val1",
        "param2": 1
      }
    }

.. code-block:: json

    {
      "test": "{{settings['param1']}}"
    }
    