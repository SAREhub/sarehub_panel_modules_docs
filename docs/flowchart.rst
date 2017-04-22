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
Podając właściwość *type* **MOŻNA** pominąć prefix *sh-*.
Możliwa jest również krótka definicja opisu pola poprzez podanie nazwy właściwości jako string,
ale w ten sposób nie ma możliwości określenia typu komponentu.

Komponenty formularza
---------------------

sh-text
```````
Pole tekstowe.
Domyślny dla typu danych: string

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "model" : {
        "type": "string",
        "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
      },
      "type": {
        "type" : "string"
      },
      "placeholder": {
        "type": "string",
        "title": "podpowiedz dla pól"
      },
      "showIf": {
        "type": "string"
      }
    }
  }

sh-textarea
```````````
Pole tekstowe dla dłuższego tekstu

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "model" : {
        "type": "string",
        "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
      },
      "type": {
        "type" : "string"
      },
      "placeholder": {
        "type": "string",
        "title": "podpowiedz dla pól"
      },
      "showIf": {
        "type": "string"
      }
    }
  }

sh-number
`````````
Pole które przyjmuje tylko wartosci liczbowe. Zawiera kontrolkę do modyfikacji wartości
Domyślny dla typu danych: integer

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "model" : {
        "type": "string",
        "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
      },
      "type": {
        "type" : "string"
      },
      "placeholder": {
        "type": "string",
        "title": "podpowiedz dla pól"
      },
      "showIf": {
        "type": "string"
      }
    }
  }

sh-checkbox
```````````
Pole podobne do przełącznika, mający tylko 2 możliwe ustawienia - właczony i wyłączony
Domyślny dla typu danych: boolean.

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "model" : {
        "type": "string",
        "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
      },
      "values": {
        "oneOf": [
          {
            "type": "object",
            "additionalProperties": true,
            "title": "Defininicja wartości do wyboru jako obiekt, gdzie wartość jest kluczem, a wartością jest zawartość atykiety"
          },
          {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "value" : {
                  "title": "wartość do wyboru"
                },
                "text": {
                  "type": "string",
                  "title": "Etykieta do wyświetlenia"
                }
              }
            }
          }
        ]
      },
      "type": {
        "type" : "string"
      },
      "showIf": {
        "type": "string"
      }
    }
  }

sh-select
`````````
Pole typu pole wyboru który pozwala na wybranie wartości z listy.

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "model" : {
        "type": "string",
        "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
      },
      "type": {
        "type" : "string"
      },
      "values": {
        "oneOf": [
          {
            "type": "object",
            "additionalProperties": true,
            "title": "Defininicja wartości do wyboru jako obiekt, gdzie wartość jest kluczem, a wartością jest zawartość atykiety"
          },
          {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "value" : {
                  "title": "wartość do wyboru"
                },
                "text": {
                  "type": "string",
                  "title": "Etykieta do wyświetlenia"
                },
                "optGroup": {
                  "type": "string",
                  "title": "Powala na na zgrupowanie wartości poprzez <optgroup> pola select"
                }
              }
            }
          }
        ]
      },
      "showIf": {
        "type": "string"
      }
    }
  }

sh-radio
````````
Pole wyboru. Zaznaczenie jednej opcji wyłącza inne opcje.

.. code-block:: json

  {
  "type": "object",
  "properties": {
    "model" : {
      "type": "string",
      "title": "odnośnik do modelu z schematu, zgodnie z notacją gdzie kolejny poziom określa się ."
    },
    "type": {
      "type" : "string"
    },
    "values": {
      "oneOf": [
        {
          "type": "object",
          "additionalProperties": true,
          "title": "Defininicja wartości do wyboru jako obiekt, gdzie wartość jest kluczem, a wartością jest zawartość atykiety"
        },
        {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "value" : {
                "title": "wartość do wyboru"
              },
              "text": {
                "type": "string",
                "title": "Etykieta do wyświetlenia"
              }
            }
          }
        }
      ]
    },
    "showIf": {
      "type": "string"
    }
  }
}

sh-fieldset
```````````
Pole służy do grupowania innych pól. Pole nie wymaga, aby zdefiniowano w nim pole *model*.

.. code-block:: json

  {
    "type": "object",
    "properties": {
      "type": {
        "type" : "string"
      },
      "showIf": {
        "type": "string"
      }
    }
  }
