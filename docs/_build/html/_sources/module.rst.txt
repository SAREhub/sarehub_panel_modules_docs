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
        }
      }
    }