###############
Obsługa zdarzeń
###############

System pozwala na określenie akcji jaka ma być wykonana przy określonych zdarzeniach systemu.

Obecnie silnik konfiguracji wspiera następujące zdarzenia:

* campaign.save - Zdarzenie podczas zapisu kampanii
* campaign.start - Zdarzenie przy uruchomieniu kampanii
* campaign.stop - Zdarzenie przy zatrzymaniu kampanii
* campaign.delete - Zdarzenie uruchomione przy usuwaniu kampanii

Aby zdefiniować akcję obsługi zdarzeń w definicji systemu należy wstawić sekcję event o strukturze:


.. code-block:: json

    {
      "events": {
        "type": "object",
        "properties": {
          "handlers": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "event" : {
                  "type" : "string",
                  "enum" : ["campaign.save", "campaign.start", "campaign.stop", "campaign.delete"]
                },
                "handler": {
                  "type": "object",
                  "properties": {
                    "type": {
                      "type": "string",
                      "title": "Nazwa akcji osbługi zdarzeń"
                    }
                  },
                "additionalProperties": true
                }
              }
            }
          }
        }
      }
    }

Akcje
=====


Zapis do REST API
-----------------

Akcja pozwala na zapis do danych do określonego API. Definicja musi być zgodna z schematem:

.. code-block:: json

    {
      "type": "object",
      "properties": {
        "event": {
          "type": "string"
        },
        "handler": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "default": "rest-api"
            },
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
                  "title" : "opcja używana do sterowania ciałem żądania. Opcja **NIE MOŻE** być używana do wysyłania żądania form-params"
                },
                "json": {
                  "title": "body wysłany jako JSON"
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
      }
    }
