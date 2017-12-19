###############
Obsługa zdarzeń
###############

System pozwala na określenie akcji jaka ma być wykonana przy określonych zdarzeniach systemu.

Aby zdefiniować akcję obsługi zdarzeń w definicji systemu lub modułu należy wstawić sekcję event o strukturze:


.. code-block:: json

    {
      "events": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "event": {
              "type": "string",
              "title": "Id zdarzenia"
            },
            "subscribers": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "type": {
                    "type": "string",
                    "title": "Typ subskrybenta"
                  }
                },
                "config": {
                  "type": "object",
                  "title" "Konfiguracja subskrybenta"
                }
              }
            }
          }
        }
      }
    }

Jeśli obsługa danego zdarzenia jest zdefiniowana w module i w systemie, zostanie użyta definicja modułu na którym jest wywoływane zdarzenia.

Obecnie silnik konfiguracji wspiera następujące zdarzenia:

1. Zdarzenia kampanii:

  * campaign.save - Zdarzenie podczas zapisu kampanii
  * campaign.start - Zdarzenie przy uruchomieniu kampanii
  * campaign.stop - Zdarzenie przy zatrzymaniu kampanii
  * campaign.delete - Zdarzenie uruchomione przy usuwaniu kampanii

2. Zdarzenia autoryzacji

  * authorization.form.send - Zdarzenie aktywowane po wysłaniu formularza autoryzacji

.. _subscribers:

Subskrybenci zdarzeń
====================
Zapis do REST API
-----------------

Akcja pozwala na wysłanie żądania do określonego API. Definicja musi być zgodna z schematem:

.. code-block:: json

    {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": [
            "rest-api"
          ]
        },
        "config": {
          "type": "object",
          "properties": {
            "clientId": {
              "type": "string",
              "title": "Id klienta, zdefiniowany w konfiguracji systemu"
            },
            "request": {
              "type": "object",
              "properties": {
                "method": {
                  "type": "string",
                  "title": "Metoda akcji w REST API",
                  "enum": [
                    "GET",
                    "POST",
                    "PUT",
                    "PATCH",
                    "DELETE"
                  ]
                },
                "action": {
                  "type": "string",
                  "title": "Url dla akcji"
                },
                "options": {
                  "type": "object",
                  "properties": {
                    "body" : {
                      "type": "string",
                      "title" : "opcja używana do sterowania ciałem żądania. Opcja **NIE MOŻE** być używana do wysyłania żądania form-params"
                    },
                    "json": {
                      "type": "object",
                      "title": "body wysłany jako JSON"
                    },
                    "form-params": {
                      "type": "object",
                      "title": "body wysłany jako application/x-www-form-urlencoded"
                    },
                    "headers": {
                      "title": "nagłówki wysłane przy żądaniu",
                      "type": "array",
                      "items": {
                        "type": "string"
                      }
                    }
                  }
                },
                "required": [
                  "type",
                  "method",
                  "action",
                  "options"
                ],
                "additionalProperties": true
              }
            },
            "onSuccess": {
              "type": "string"
            },
            "onError": {
              "type": "string"
            }
          }
        }
      }
    }
