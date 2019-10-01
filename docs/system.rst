######
System
######

Definicja systemu jest podstawową częścią integracji i stanowi wejście dla reszty konfiguracji w pliku *system.json*:

+---------------+--------------------------------------------------------+----------+----------------------------------------------+
|   Parametr    |                          Opis                          | Wymagane |                    Uwagi                     |
+===============+========================================================+==========+==============================================+
| id            | Unikalny identyfikator systemu                         | TAK      | Maksymalnie 32 znaki **A-Z, a-z, 0-9, -, _** |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| name          | Nazwa systemu wyświetlana w panelu integracji          | TAK      | Maksymalnie 255 znaków                       |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| logo          | Url do loga wyświetlanego w panelu integracji          | NIE      | Maksymalnie 255 znaków                       |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| description   | Opis systemu                                           | NIE      |                                              |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| version       | Wersja konfiguracji integracji                         | TAK      | Wersja w formacie Major.Minor.Patch          |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| authorization | Obiekt zawierający konfigurację autoryzacji integracji | TAK      | Opis szczegółowy poniżej                     |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+
| modules       | Tablica zawierająca definicje modułów                  | TAK      | Lista definicji :doc:`modułów </module>`     |
+---------------+--------------------------------------------------------+----------+----------------------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "id_systemu",
      "name": "Nazwa systemu",
      "logo": "test.pl/logo.png",
      "description": "Opis systemu",
      "version": "1.0.0",
      "config" : {},
      "modules": []
    }

Autoryzacja systemu - authorization
===================================

Konfiguracja określa kilka możliwości integracji i autoryzacji systemu zewnętrznego.

+---------------+--------------------------------------------------------+----------+------------------------------------------+
| Parametr      | Opis                                                   | Wymagane | Uwagi                                    |
+===============+========================================================+==========+==========================================+
| type          | Określenie typu                                        | TAK      | Typy opisane poniżej                     |
+---------------+--------------------------------------------------------+----------+------------------------------------------+
| config        | Dodatkowa konfiguracja dla typu autoryzacji            | NIE      | Zależnie od typu                         |
+---------------+--------------------------------------------------------+----------+------------------------------------------+
| restrictions  | Ustawienia ograniczeń dla integracji                   | NIE      | Opisane poniżej                          |
+---------------+--------------------------------------------------------+----------+------------------------------------------+

.. code-block:: json

    {
      "type": "authorization_type",
      "config": {},
      "restrictions": {}
    }

Typy autoryzacji - type
-----------------------

- Brak - none

Przy typie *none* użytkownik nie może sam wykonać integracji systemu w jego koncie i musi być uruchomiona ręcznie przez administratora lub automatycznie przy włączeniu integracji.
Pole *config* jest ignorowane. 

- OAUTH 2 - oauth

Pozwala na autoryzację systemu SAREhub z integrowanym systemem którą może wykonać użytkownik systemu. Wykorzystuje protokół  `oauth2 <https://oauth.net/2/>`_.
Konfiguracja typu **MUSI** zawierać następujące parametry:

+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
|        Parametr         |                                       Opis                                        | Wymagane |                                  Uwagi                                   |
+=========================+===================================================================================+==========+==========================================================================+
| clientId                | Id określone w konfiguracji serwera OAUTH                                         | TAK      | Musi byc zgodne z serwerem OAUTH                                         |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
| clientSecret            | Secret określony w konfiguracji serwera OAUTH                                     | TAK      | Musi byc zgodne z serwerem OAUTH                                         |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
| urlAuthorize            | Url pod którym uruchamiana jest autoryzacja(panel logowania)                      | TAK      |                                                                          |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
| urlAccessToken          | Url pod którym SAREhub może pobrać nowe tokeny                                    | TAK      |                                                                          |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
| urlResourceOwnerDetails | Url pod którym SAREhub może pobrać dane konta z którym jest wykonywana integracja | TAK      |                                                                          |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+
| provider                | Określenie silnika integracji oauth                                               | NIE      | Domyślnie *sarehub_generic* lub nieokreślone, chyba, że wskazano inaczej |
+-------------------------+-----------------------------------------------------------------------------------+----------+--------------------------------------------------------------------------+

Przykład:

.. code-block:: json

    {
      "clientId": "idKlientaOauth",
      "clientSecret": "secretKlientaOauth",
      "urlAuthorize": "https://system.pl/oauth/authorize",
      "urlAccessToken": "https://system.pl/oauth/token",
      "urlResourceOwnerDetails": "https://system.pl/oauth/account"
    }

Restrykcje dla integracji - restrictions
----------------------------------------

Pozwala na wskazanie ograniczeń dla danej integracji

+----------+--------------------------------------------------------------------------------------+----------+-------+
| Parametr |                                         Opis                                         | Wymagane | Uwagi |
+==========+======================================================================================+==========+=======+
| count    | Ograniczenie liczby integracji jakie może wykonać użytkownik z integrowanym systemem | Nie      |       |
+----------+--------------------------------------------------------------------------------------+----------+-------+

Przykład:

.. code-block:: json

    {
      "count": 1
    }