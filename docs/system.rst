######
System
######

Definicja systemu jest podstawową cześcią integracji i stanowi wejście dla reszty konfiguracji i **MUSI** być zdefiniowany według schematu:

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
          "authorization": {
            "type": "object"
            "properties": {
              "handler": {
                "title": "sposób obsługi integracji z systemem",
                "type": "object"
              },
              "restrictions" : {
                "title" : "lista warunków dla integracji",
                "type": "array"
              }
            }
          }
          "modules": {
            "title": "Lista modułów systemu"
            "type": "array",
            "items": {
                "type" : "object"
            }
          }
    }

Autoryzacja systemu
===================

Sekcja konfiguracji określa akcję jaka ma być wykonana gdy użytkownik chce włączyć integrację z systemem w swoim koncie po raz pierwszy.

Obecnie dostępne są następujące sposoby integracji:

OAUTH2
------

Typ pozwala na autoryzację systemu SAREhub w integrowanym systemie, wykorzystując protokół  `oauth2 <https://oauth.net/2/>`_.
Parametry clientId, clientSecret, urlAuthorize, urlAccessToken, urlResourceOwnerDetails muszą być ustawione w zewnętrznym systemie.
W systemie zewnętrznym musi być rówież ustawiony powrotny adres url, na który zostanie przekierowany użytkownik po udanej lub nieudanej authoryzacji.
Link ten jest generowany w systemie SAREhub i jest w formacie:
*https://my.sarehub.com/pl/oauth/authorize/id_systemu* gdzie id_systemu brany jest z konfiguracji.

.. code-block:: json

    {
      "type": "object",
      "propeties": {
        "type": {
          "type": "string",
          "default": "oauth2",
          "config": {
            "type": "object",
            "properties": {
              "clientId": {
                "type": "string",
                "title": "clientId zgodny z OAUTH2, nadany w systemie zewnętrznym"
              },
              "clientSecret": {
                "type": "string",
                "title": "clientSecret zgodny z OAUTH2, nadany w systemie zewnętrznym"
              },
              "urlAuthorize": {
                "type": "string",
                "title": "url do zewnętrzego systemu, pod którym rozpoczyna się integracja"
              },
              "urlAccessToken": {
                "type": "string",
                "title": "url do zewnętrzego systemu, pod którym odświeżany jest token systemu"
              },
              "urlResourceOwnerDetails": {
                "type": "string"
              }
            },
            "required": [
              "clientId",
              "clientSecret",
              "urlAuthorize",
              "urlAccessToken"
            ]
          }
        }
      }
    }
