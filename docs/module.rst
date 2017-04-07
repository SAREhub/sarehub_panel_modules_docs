######
Moduły
######

Każdy system **MUSI** posiadać definicję dla co najmniej jednego modułu. A każdy moduł musi być zdefiniowany według schematu:

.. code-block:: json

    {
      "type": "object",
      "properties": {
          "id": {
            "title": "unikalny identyfikator modułu",
            "pattern" : "^[A-Za-z\d]+$",
            "type": "string"
          },
          "label": {
            "title": "nazwa modułu wyświetlana w panelu",
            "type": "string"
          },
          "handlers": {
            "title": "",
            "type": "object",
            "properties" : {
              "campaignStart": {
                "title": "akcja do wykonania podczas uruchomienia kampanii",
                "type" : "object"
              },
              "campaignStop": {
                "title": "akcja do wykonania podczas zatrzymania kampanii",
                "type" : "object"
              },
              "campaignSave" : {
                "title": "akcja do wykonania podczas zapisu kampanii",
                "type" : "object"
              },
              "campaignDelete" : {
                "title": "akcja do wykonania podczas kasowania kampanii",
                "type" : "object"
              }
            }
          }
      }
    }

Obsługa zdarzeń - handlers
==========================
