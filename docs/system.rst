######
System
######

Definicja systemu jest podstawową cześcią koniguracji i stanowi wejście dla silnika konfiguracji i **MUSI** być zdefiniowany według schematu:

.. code-block:: json

    {
      "type": "object",
      "properties": {
          "id": {
            "title": "unikalny identyfikator systemu",
            "pattern" : "^[A-Za-z\d]+$",
            "type": "string"
          },
          "label": {
            "title": "nazwa systemu wyświetlana w panelu",
            "type": "string"
          }
          "integration": {
            "title": "",
            "type": "object"
            "properties": {
              "handler": {
                "title": "sposób obsługi integracji z systemem",
                "type": "string"
              },
              "restrictions" : {
                "title" : "lista warunków dla integracji"
                "type": "array"
              }
            }
          }
          "modules": {
            "title": "Lista modułów systemu"
            "type": "array",
          }
    }
