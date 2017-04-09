########################
Wstępne parsowanie pliku
########################

Konfiguracja przed właściwym użyciem przez silnik jest wstępnie parsowany.
Ten proces może być kontrolowany przy użyciu dyrektyw preprocesora.

Dyrektywy parsowania
====================
Dyrektywy warunkowe
-------------------

Dyrektywy warunkowe **MUSZĄ** być zdefiniowane wg. poniższego schematu:

.. code-block:: json

    {
      "definitions": {
        "conditionsDirectives": {
          "type": "object",
          "properties": {
            "items": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "if": {
                    "oneOf": [
                      {
                        "type": "string",
                        "title": "warunek do spełnienia"
                      },
                      {
                        "type": "array",
                        "title": "tablica składowych warunku do spełnienia",
                        "items": {
                          "type": "object",
                          "properties": {
                            "condition": {
                              "type": "string",
                              "title": "warunek do spełnienia"
                            },
                            "type": {
                              "type": "string",
                              "title": "typ łączenia warunku",
                              "enum": [
                                "AND",
                                "OR"
                              ]
                            }
                          }
                        }
                      }
                    ]
                  },
                  "content": {
                    "title": "zawartość ktora ma być użyta jeśli zostanie spełnione wyrażenie"
                  }
                }
              }
            }
          }
        }
      },
      "oneOf": [
        {
          "@anyOf": {
            "$ref": "#/definitions/conditionsDirectives"
          },
          "@oneOf": {
            "$ref": "#/definitions/conditionsDirectives"
          }
        }
      ]
    }

@anyOf
++++++

Dyrektywa pozwala na zdefiniowane listy konfiguracji które mają być wybrane jeśli zostaną spełnione określone warunki.

@oneOf
++++++

Dyrektywa pozwala na wybranie wyłącznie jednego wariantu po spełnieniu warunku.
Przy parsowaniu jeden warunek **MUSI** zostać spełniony, więc zalecane jest,
aby ostatnii warunek był określony przez wyrażenie true. Jeśli warunek zostanie spełniony parsowanie zostanie zatrzymane.


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

expresion
---------

Preprocesor pozwala na wstawienie wyniku wyrażenia lub zmiennych we wskazane miejsce.
Wyrażenie **MUSI** być wstawione w postaci *{{wyrażenie}}*.
W jednym polu może znajdować się wiele bloków wyrażeń.

Przykład:

.. code-block:: json

    {
        "exp1" : "{{settings[0]}} {{settings[1]}}",
        "exp2" : "{{settings[0] * settings[1}}"
    }

@formatter
----------

Dyrektywa pozwala na przeformatowanie bloku konfiguracji z jednego typu na drugi.

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
