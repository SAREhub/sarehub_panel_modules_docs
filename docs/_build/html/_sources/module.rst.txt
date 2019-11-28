.. _module_ref:

######
Moduły
######

Każdy system **MUSI** posiadać definicję dla co najmniej jednego modułu. 

+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
|     Parametr     |                                 Opis                                  | Wymagane |                          Uwagi                          |
+==================+=======================================================================+==========+=========================================================+
| name             | Unikalny nazwa modułu                                                 | TAK      | Maksymalnie 32 znaki w formacie A-Z, a-z, 0-9, -, _     |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| label            | Nazwa modułu wyświetlana w panelu                                     | TAK      | Maksymalnie 255 znaków                                  |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| interface_config | Obiekt określający zmienne do wykorzystania przy budowaniu formularza | NIE      | Opis szczegółowy poniżej                                |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| event            | Obiekt zawierający konfigurację obsługi zdarzeń                       | NIE      | Opis szczegółowy poniżej                                |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| functionality    | Obiekt zawierający konfigurację funkcjonalności                       | TAK      | Lista definicji :doc:`funkcjonalności </functionality>` |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| save             | Obiekt zawierający konfigurację zapisu modułu                         | TAK      | Opis szczegółowy poniżej                                |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+
| translation      | Obiekt zawierający zbiór tłumaczeń używanych w module                 | TAK      | Opis szczegółowy poniżej                                |
+------------------+-----------------------------------------------------------------------+----------+---------------------------------------------------------+

Przykład:

.. code-block:: json

    {
      "name": "nazwa_modułu",
      "label": "Wyświetlana nazwa modułu",
      "interface_config": {},
      "event" : {},
      "functionality": {},
      "save": {},
      "translation": {}
    }

Definicja zmiennych - interface_config
**************************************

Służy głównie do zdefiniowania zmiennych które mają zostać wstawione w definicję formularza funkcjonalności i umożliwia pobranie zasobu z integrowanego systemu.


+--------------------+------------------------------------------------------------------------------------------+----------+------------------------------------------------------------------------------------------------------------------------+
|      Parametr      |                                           Opis                                           | Wymagane |                                                         Uwagi                                                          |
+====================+==========================================================================================+==========+========================================================================================================================+
| apiGlobals         | Lista zmiennych to zastąpienia w definicjach zmiennych **apiVariable**                   | NIE      | Tablica gdzie wartością jest nazwa zmiennej w formacie **A-Z, _**, a wartoscią pola jest wartość stałej                |
+--------------------+------------------------------------------------------------------------------------------+----------+------------------------------------------------------------------------------------------------------------------------+
| apiVariable        | Lista zmiennych to zastąpienia w definicjach formularzy funkcjonalności                  | NIE      | Tablica gdzie wartością jest nazwa zmiennej w formacie **A-Z, _**, a wartoscią definicja zapytania do zewnętrznego API |
+--------------------+------------------------------------------------------------------------------------------+----------+------------------------------------------------------------------------------------------------------------------------+
| prependApiVariable | Pozwala na wstawienie wartości do zmiennych z **apiVariable** przed tymi pobranymi z API | NIE      | Tablica gdzie wartością jest nazwa zmiennej w formacie **A-Z, _**                                                      |
+--------------------+------------------------------------------------------------------------------------------+----------+------------------------------------------------------------------------------------------------------------------------+
| appendApiVariable  | Pozwala na wstawienie wartości do zmiennych z **apiVariable** za tymi pobranymi z API    | NIE      | Tablica gdzie wartością jest nazwa zmiennej w formacie **A-Z, _**                                                      |
+--------------------+------------------------------------------------------------------------------------------+----------+------------------------------------------------------------------------------------------------------------------------+


Definicja zapytania - apiVariable
=================================

Parametry definicji:

+-----------+----------------------------------+----------+-------------------------------------+
| Parametr  |               Opis               | Wymagane |                Uwagi                |
+===========+==================================+==========+=====================================+
| url       | Url do zasobu                    | TAK      |                                     |
+-----------+----------------------------------+----------+-------------------------------------+
| method    | Metoda HTTP                      | TAK      | GET, POST itp.                      |
+-----------+----------------------------------+----------+-------------------------------------+
| converter | Metoda parsująca odpowiedz z API | NIE      | Wymaga ustalenia z zespołem SAREhub |
+-----------+----------------------------------+----------+-------------------------------------+
| options   | Obiekt opcji zapytania           | TAK      | Opis szczegółowy poniżej            |
+-----------+----------------------------------+----------+-------------------------------------+

Opcje zapytania:

+-------------+-------------------------------------------+----------+---------------------------------------------------------------------------+
|  Parametr   |                   Opis                    | Wymagane |                                   Uwagi                                   |
+=============+===========================================+==========+===========================================================================+
| headers     | Tablica nagłówków zapytania               | NIE      |                                                                           |
+-------------+-------------------------------------------+----------+---------------------------------------------------------------------------+
| form_params | Tablica parametrów wysłana w żądaniu POST | NIE      | Tablica gdzie kluczem jest nazwa parametru, a wartością wartość parametru |
+-------------+-------------------------------------------+----------+---------------------------------------------------------------------------+

Przykład:

.. code-block:: json

    {
      "...": "...",
      "VAR_MODULE": {
        "url": "http://test.pl/api/test",
        "method": "POST",
        "converter": "convertVarModule",
        "options": {
          "headers": [
            "application/x-www-form-urlencoded"
          ],
          "form_params": {
            "param1": "param2"
          }
        }
      },
      "...": "..."
    }


Definicja zdarzeń - event
*************************

Pole posiada tylko jeden parametr - *listeners*, który pozwala na określenie listy wywołań funkcji w momencie wywołania przez system SAREhub określonych zdarzeń.

Parametry definicji wywołania:

+----------+----------------------------------+----------+----------------------+
| Parametr |               Opis               | Wymagane |        Uwagi         |
+==========+==================================+==========+======================+
| type     | Nazwa funkcji do wywołania       | TAK      | Typy opisane poniżej |
+----------+----------------------------------+----------+----------------------+
| config   | Dodatkowa konfiguracja wywołania | TAK      |                      |
+----------+----------------------------------+----------+----------------------+

Dostępne funkcje nasłuchujące
=============================

apiCallListener
---------------

Funkcja pozwala na wywołanie metody API.

Parametry konfiguracji:

+----------+------------------------------+----------+-------+
| Parametr |             Opis             | Wymagane | Uwagi |
+==========+==============================+==========+=======+
| client   | Identyfikator klienta Guzzle | TAK      |       |
+----------+------------------------------+----------+-------+
| request  | Konfiguracja żądania do API  | TAK      |       |
+----------+------------------------------+----------+-------+

Parametry konfiguracji żądania

+----------+---------------+----------+-------------------------+
| Parametr |     Opis      | Wymagane |          Uwagi          |
+==========+===============+==========+=========================+
| method   | Metoda HTTP   | TAK      | GET, POST itp.          |
+----------+---------------+----------+-------------------------+
| url      | Url do zasobu | TAK      |                         |
+----------+---------------+----------+-------------------------+
| options  | Opcję żądania | TAK      | Opcję zgodne z Guzzle_. |
+----------+---------------+----------+-------------------------+

.. _Guzzle: http://docs.guzzlephp.org/en/stable/request-options.html

Dostępne zdarzenia
^^^^^^^^^^^^^^^^^^

campaign.started
""""""""""""""""

Zdarzenie wyłowywane gdy zostanie uruchomiona kampania.


Przykład:

.. code-block:: json

    {
      "event": {
          "listeners": [
            {"...": "..."},
            {
              "type": "apiCallListener",
              "config": {
                "client": "test_api",
                "request": {
                  "method": "POST",
                  "url": "resource/1",
                  "options": {
                    "name": "test"
                  }
                }
                "event": [
                  {
                    "name": "campaign.started",
                  }
                ]
              }
            },
            {"...": "..."},
          ]
        }
    }


entityChangedListener
---------------------

Funkcja pozwala na przeładowanie kampanii po wywołaniu zdarzenia.

Parametry konfiguracji:

+---------------+------------------------------------------------------------+----------+-------+
|   Parametr    |                            Opis                            | Wymagane | Uwagi |
+===============+============================================================+==========+=======+
| functionality | Lista funkcjonalności dla których ma być aktywne wywołanie | TAK      |       |
+---------------+------------------------------------------------------------+----------+-------+
| event         | Lista zdarzeń do nasłuchiwania                             | TAK      |       |
+---------------+------------------------------------------------------------+----------+-------+


Dostępne zdarzenia
^^^^^^^^^^^^^^^^^^

entity.changed
""""""""""""""

Zdarzenie wyłowywane gdy zostanie zaktualizowana określona encja.

Konfiguracja zdarzenia:

+----------+-----------------------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------+
| Parametr |                        Opis                         | Wymagane |                                                            Uwagi                                                            |
+==========+=====================================================+==========+=============================================================================================================================+
| entity   | Lista encji przy których ma zostac wywołana funkcja | TAK      | Tablica gdzie kluczem jest nazwa encji, a wartoscią jest tablica identyfikatorów pól z definicji formularza funkcjonalności |
+----------+-----------------------------------------------------+----------+-----------------------------------------------------------------------------------------------------------------------------+


Lista encji:

+-------------------------+----------------------------------------------------------+
|          Encja          |                           Opis                           |
+=========================+==========================================================+
| testGroup               | Grupy testowe, należy wstawić null jako pierwszy element |
+-------------------------+----------------------------------------------------------+
| notificationsTemplate   | Szablon powiadomienia webpush                            |
+-------------------------+----------------------------------------------------------+
| personalizationTemplate | Szablon personalizacji                                   |
+-------------------------+----------------------------------------------------------+


Przykład:

.. code-block:: json

    {
      "event": {
          "listeners": [
            {"...": "..."},
            {
              "type": "entityChangedListener",
              "config": {
                "functionality": [
                  "notifications"
                ],
                "event": [
                  {
                    "name": "entity.changed",
                    "config": {
                      "entity": {
                        "testGroup": [
                          null
                        ],
                        "notificationsTemplate": [
                          "notification_template_id"
                        ],
                        "personalizationTemplate": [
                          "web_push_personalization"
                        ]
                      }
                    }
                  }
                ]
              }
            },
            {"...": "..."},
          ]
        }
    }


integrationIdentityListener
---------------------------

Funkcja pozwala na modyfikację identyfikatorów SAREweb

Parametry konfiguracji:

+---------------+------------------------------------------------------------+----------+-------+
|   Parametr    |                            Opis                            | Wymagane | Uwagi |
+===============+============================================================+==========+=======+
| event         | Lista zdarzeń do nasłuchiwania                             | TAK      |       |
+---------------+------------------------------------------------------------+----------+-------+


Dostępne zdarzenia
^^^^^^^^^^^^^^^^^^

integration.completed
"""""""""""""""""""""

Zdarzenie wyłowywane po poprawnej integracji systemu.


Przykład:

.. code-block:: json

    {
      "event": {
          "listeners": [
            {"...": "..."},
            {
              "type": "integrationIdentityListener",
              "config": {
                "event": [
                  {
                    "name": "entity.changed",
                    "config": {}
                  }
                ]
              }
            },
            {"...": "..."},
          ]
        }
    }

integration.deleted
"""""""""""""""""""""

Zdarzenie wyłowywane po poprawnym usunięciu integracji.


Przykład:

.. code-block:: json

    {
      "event": {
          "listeners": [
            {"...": "..."},
            {
              "type": "integrationIdentityListener",
              "config": {
                "event": [
                  {
                    "name": "entity.deleted",
                    "config": {}
                  }
                ]
              }
            },
            {"...": "..."},
          ]
        }
    }


Zapis modułu - save
*******************

Służy do określenia w jaki sposób konfiguracja modułu ma być zapisana do wykożystania w flow kampanii.
Zapis modułu wywoływany jest w momencie uruchomienia kampanii.

+----------+---------------------+----------+------------------+
| Parametr |        Opis         | Wymagane |      Uwagi       |
+==========+=====================+==========+==================+
| type     | Typ obsługi zapisu  | TAK      | Opisane poniżej  |
+----------+---------------------+----------+------------------+
| config   | Konfiguracja zapisu | TAK      | Zależnie od typu |
+----------+---------------------+----------+------------------+


Wywołanie API - api-call
========================

Pozwala na zapis konfiguracji modulu w określonym API.

Parametry konfiguracji:

+----------+------------------------------+----------+-------+
| Parametr |             Opis             | Wymagane | Uwagi |
+==========+==============================+==========+=======+
| client   | Identyfikator klienta Guzzle | TAK      |       |
+----------+------------------------------+----------+-------+
| request  | Konfiguracja żądania do API  | TAK      |       |
+----------+------------------------------+----------+-------+

Parametry konfiguracji żądania

+----------+---------------+----------+-------------------------+
| Parametr |     Opis      | Wymagane |          Uwagi          |
+==========+===============+==========+=========================+
| method   | Metoda HTTP   | TAK      | GET, POST itp.          |
+----------+---------------+----------+-------------------------+
| url      | Url do zasobu | TAK      |                         |
+----------+---------------+----------+-------------------------+
| options  | Opcję żądania | TAK      | Opcję zgodne z Guzzle_. |
+----------+---------------+----------+-------------------------+

.. _Guzzle: http://docs.guzzlephp.org/en/stable/request-options.html

Przykład:

.. code-block:: json

    {
      "save": {
        "type": "api-call",
        "config": {
          "client": "test_client",
          "request": {
            "method": "POST",
            "url": "resource/1",
            "options": {
              "json": {
                "name": "test"
              }
            }
          }
        }
      }
    }

Zapis zależny od konfiguracji - multi
=====================================

Pozwala na określenie konfiguracji zależnie od funkcjonalności:

Przykład:

.. code-block:: json

    {
      "save": {
        "type": "multi",
        "config": {
          "functionality": {
            "func1": {
              "type": "api-call",
              "config": {}
            },
            "func2": {
              "type": "mongo-api",
              "config": {}
            },
          }
        }
      }
    }


Zapis do MongoDB - mongo-api
============================

Pozwala na zapis konfiguracji modulu w bazie MongoDB. Typ używany głównie w wewnętrznych modułach systemu SAREhub.

Parametry konfiguracji:

+----------+------------------------+----------+------------------------------+
| Parametr |          Opis          | Wymagane |            Uwagi             |
+==========+========================+==========+==============================+
| module   | Nazwa modułu           | TAK      |                              |
+----------+------------------------+----------+------------------------------+
| rule     | Reguła w formacie JSON | TAK      | Reguły sa łączone dla modułu |
+----------+------------------------+----------+------------------------------+

Przykład:

.. code-block:: json

    {
      "save": {
        "type": "mongo-api",
        "config": {
          "module": "test_module",
          "rule": {
            "event": "test",
            "condition": "test"
          }
        }
      }
    }


Tłumaczenia - translation
*************************

Definicja tłumaczeń do użycia w konfiguracji formularzy.

Przykład: 

.. code-block:: json

    {
      "translation": {
        "pl": {
          "name": "nazwa",
        }
      }
    }


Tłumaczenia w formularzach można użyć poprzez dyrektywę %TRANSLATE|klucz_w_translacji%.

Przykład: 

.. code-block:: json

    {
      "label": "%TRANSLATE|name"
    }

