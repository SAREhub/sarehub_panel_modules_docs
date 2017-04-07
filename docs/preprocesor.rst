########################
Wstępne parsowanie pliku
########################

Większość fragmentów konfiguracji przed właściwą analizą przez silnik jest wstępnie parsowany przez preprocesor szablonu.

Preprocesors
============

anyOf
-----

Preprocesor pozwala na zdefiniowanie różnych fragmentów konfiguracji, które zostaną użyte jeśli zostaną spełnione ich warunki.

Preprocesor **MUSI** być zdefiniowany według schematu:

.. code-block:: json

  {
    "@anyOf": {
        "type": "object",
        "properties": {
            "items": {
              "title": "lista warunków",
              "type": "array"
            }
        }
      }
    }

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "if" : {
        "title" : "wyrażenie warunkowe lub wartość true",
        "type": "string"
      },
      "content" : {
        "title" : "zawartość ktora ma być użyta jeśli zostanie spełnione wyrażenie"
      }
    }
  }

oneOf
-----

Preprocesor pozwala na wybranie wyłącznie jednego wariantu po spełnieniu warnku. Przy parsowaniu jeden warunek **MUSI** zostać spełniony, więc zalecane jest,
aby ostatnii warunek był określony przez wyrażenie true. Jeśli warunek zostanie spełniony parsowanie zostanie zatrzymane.

Preprocesor **MUSI** być zdefiniowany według schematu:

.. code-block:: json

    {
      "@oneOf": {
          "type": "object",
          "properties": {
              "items": {
                "title": "lista warunków",
                "type": "array"
              }
          }
        }
      }

.. code-block:: json

    {
      "type": "object",
      "properties": {
        "if" : {
          "title" : "wyrażenie warunkowe lub wartość true",
          "type": "string"
        },
        "content" : {
          "title" : "zawartość ktora ma być użyta jeśli zostanie spełnione wyrażenie"
        }
      }
    }

expresion
---------

Preprocesor pozwala na wstawienie wyniku wyrażenia lub zmiennych dla wartości pól typu string. Wyrażenie **MUSI** być wstawione w postaci {{wyrażenie}}.
W jednym polu może znajdować się wiele bloków wyrażeń.

formatter
---------

Preprocesor pozwala na przeformatowanie bloku konfiguracji z jednego typu na drugi.

Definicja preprocesora **MUSI** być zdefiniowana wg schematu:

.. code-block:: json

    {
      "@formatter": {
          "type": "object",
          "properties": {
              "type": {
                "title": "type formatu",
                "type": "string"
              }
          }
        }
      }

Obecnie jedynym dostępnym typem jest *string* który zamienia obiekt lub tablicę do typu tekstowego.

Typ **MUSI** być zdefiniowany wg schematu:

.. code-block:: json

    {
      "@formatter": {
          "type": "object",
          "properties": {
              "type": {
                "title": "type formatu",
                "type": "string"
              },
              "concat": {
                "title": "opcjonalny łącznik",
                "type": "string"
              }
          }
        }
      }
