######
System
######

Definicja systemu jest podstawową cześcią integracji i stanowi wejście dla reszty konfiguracji i **MUSI** być zdefiniowana według schematu:

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
            "title": "nazwa bloczka funkcjonalności",
            "type": "string"
          },
          "tags": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "version": {
            "title": "Wersja konfiguracji w formacie 'Major (numer główny).Minor (numer dodatkowy).Release (numer wydania)'",
            "type": "string"
          },
          "clients": {
            "type": "object",
            "title": "Definicja klientów REST API"
          },
          "authorization": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "handler": {
                  "title": "Sposób obsługi integracji z systemem",
                  "type": "object"
                },
                "restrictions" : {
                  "title" : "Lista warunków dla integracji",
                  "type": "array"
              }
            }
          },
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

Sekcja konfiguracji określa akcję autoryzacji jaka ma być wykonana gdy użytkownik chce włączyć integrację z systemem w swoim koncie po raz pierwszy.

Obecnie dostępne są następujące sposoby autoryzacji integracji:

OAUTH2
------

Pozwala na autoryzację systemu SAREhub z integrowanym systemem, wykorzystując protokół  `oauth2 <https://oauth.net/2/>`_.
Parametry clientId, clientSecret, urlAuthorize, urlAccessToken, urlResourceOwnerDetails muszą być ustawione w zewnętrznym systemie.
W systemie zewnętrznym musi być rówież ustawiony powrotny adres url, na który zostanie przekierowany użytkownik po udanej lub nieudanej autoryzacji.
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

Klienci REST API
================

Jeśli konfiguracja wymaga odwołań do zewnętrznego REST API to **MUSIMY** zdefiniować odwołanie w polu *clients* w definicji systemu. Definicja musi być zdefiniowana w postaci obiektu, gdzie kluczem jest unikalny id api, a wartością jest obiekt konfiguracji określony wg schematu:

.. code-block:: json

    {
      "type": "object",
      "properties": {
          "baseUrl": {
            "title": "Bazowy url dla zapytań",
            "type": "string"
          },
          "authorization": {
            "title": "Typ autoryzacji w api",
            "type": "string"
          }
    }
