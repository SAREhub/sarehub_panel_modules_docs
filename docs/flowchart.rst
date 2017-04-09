###################
Flowchart - Bloczki
###################

Każdy moduł **MOŻE** zawierać jeden lub kilka bloczków dostępnych na schemacie kampanii.
Bloczki stanowią najbardziej podstawą cześć modułu systemu i powinny zawierać tylko jedną główną funkcjonalność.
Każdy bloczek składa się z reprezentcji graficznej na flowcharcie oraz formularza ustawień.

Konfigurację bloczka **MUSI** być zdefiniowana wg podanego schematu:

.. code-block:: json

  {
    "type": "object",
    "properties": {
        "id": {
          "title": "unikalny identyfikator bloczka",
          "type": "string"
        },
        "label": {
          "title": "nazwa bloczka wyświetlana w panelu",
          "type": "string"
        },
        "flowchart": {
          "type": "object",
          "properties": {
            "node": {
              "title": "Opisuje wygląd bloczka na schemacie kampanii",
              "type": "object"
            },
            "form": {
              "title": "Opisuje formularz ustawień bloczka",
              "type": "object",
              "properties": {
                "structure": {
                  "type": "object",
                  "title": "Opis struktury formularza określający interfejs użytkownika"
                },
                "schema": {
                  "type": "object",
                  "title": "Schemat danych formularza"
                }
              }
            }
          }
        }
      }
    }
  }

Schemat danych - schema
=======================
Struktura konfiguracji opisuje właściwości obiektu wynikowego powstającego po wypełnieniu formularza ustawień i **MUSI** być zgodna z specyfikacją `JSON Schema <http://json-schema.org/>`_ w wersji 5 z dnia 2016-10-13.

Przykład schematu danych:

.. code-block:: json

  {
    "type": "object",
    "definitions": {
        "delayType" : {
          "oneOf": [
            {
              "properties": {
                "amount": {
                  "type": "integer",
                  "minimum": 0,
                  "maximum": 60
                },
                "period": {
                  "type": "integer",
                  "enum": [
                        1,
                        60,
                        3600,
                        86400,
                        604800,
                        18144000
                  ]
                }
              },
              "required" : ["amount", "period"]
            },
            {
              "properties": {
                "to": {
                  "type": "string"
                }
              },
              "required" : ["to"]
            }
          ]
        }
      }
  }

Struktura formularza - structure
================================
Właściwość jest tablicą która opisuje kolejne pola formularza które zostaną wyświetlone użytkownikowi i jest ściśle związana z schematem danych.
Pole formularza **MOŻE** być zdefiniowany jako obiekt zawierający właściwości *model* gdzie należy podać nazwę właściwości z schematu danych formularza.
Drugim wymaganym polem jest *type* które jawnie określa wybrany komponent formularza.
Możliwa jest również krótka definicja opisu pola poprzez podanie nazwy właściwości jako string,
ale w ten sposób nie ma możliwości określenia typu komponentu.

Komponenty formularza
---------------------

sh-text
```````
Pole tekstowe.
Domyślny dla typu danych: string

sh-textarea
```````````
Pole tekstowe dla dłuższego tekstu

sh-number
`````````
Pole które przyjmuje tylko wartosci liczbowe. Zawiera kontrolkę do modyfikacji wartości
Domyślny dla typu danych: integer

sh-checkbox
```````````
Pole podobne do przełącznika, mający tylko 2 możliwe ustawienia - właczony i wyłączony
Domyślny dla typu danych: boolean.

sh-select
`````````
Pole typu pole wyboru który pozwala na wybranie wartości z listy.

sh-radio
````````

sh-group
````````

sh-fieldset
```````````
