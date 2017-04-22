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
          "clients": {
            "type": "object",
            "title": "Definicja klientów REST API"
          },
          "vars": {
            "type": "object",
            "title": "Definicja zmiennych do użycia w konfiguracji"
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

Klienci REST API
================

Jeśli konfiguracja wymaga odwołań do zewnętrznego REST API to **MUSI** być ono zdefiniowane w polu *clients* w definicji systemu. Definicja musi być zdefiniowana w postaci obiektu, gdzie kluczem jest unikalny id api, a wartością jest obiekt konfiguracji określony wg schematu:

.. code-block:: json

    {
      "type": "object",
      "properties": {
          "baseUrl": {
            "title": "Bazowy url dla zapytań",
            "type": "string"
          },
          "authorization": {
            "title": "typ autoryzacji w api",
            "type": "string"
          }
    }

Definicja zmiennych
===================
Wszystkie zmienne które mają byc użyte w konfiguracji należy zdefiniować w polu *vars* w definicji systemu, modułu i bloczka. 
Obiekt konfiguracji zmiennej **MUSI** być zdefiniowany wg schematu:

.. code-block:: json

    {
      "type": "object",
      "properties": {
          "type": {
            "title": "Typ zmiennej",
            "type": "string"
          },
          "value": {
            "title": "wartość zmiennej"
          },
          "allowOverwrite": {
            "type": "bool",
            "title": "Pole określa czy zmienna może być nadpisana w późniejszej konfiguracji
          },
          "allowPanelOverwrite": {
            "type": "bool",
            "title": "Pole określa czy zmienna może być nadpisana dla konta w SAREhub
          }
    }
