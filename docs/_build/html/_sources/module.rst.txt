.. _module_ref:
######
Moduły
######

Każdy system **MUSI** posiadać definicję dla co najmniej jednego modułu. 

+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
|     Parametr     |                                 Opis                                  | Wymagane |                           Uwagi                           |
+==================+=======================================================================+==========+===========================================================+
| name             | Unikalny nazwa modułu                                                 | TAK      | Maksymalnie 32 znaki w formacie A-Z, a-z, 0-9, -, _       |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| label            | Nazwa modułu wyświetlana w panelu                                     | TAK      | Maksymalnie 255 znaków                                    |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| interface_config | Obiekt określający zmienne do wykorzystania przy budowaniu formularza | NIE      | Opis szczegółowy poniżej                                  |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| event            | Obiekt zawierający konfigurację obsługi zdarzeń                       | NIE      | Opis szczegółowy poniżej                                  |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| functionality    | Obiekt zawierający konfigurację funkcjonalności                       | TAK      | Lista definicji funkcjonalności (bloczków na flowcharcie) |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| save             | Obiekt zawierający konfigurację zapisu modułu                         | TAK      | Opis szczegółowy poniżej                                  |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+
| translation      | Obiekt zawierający zbiór tłumaczeń używanych w module                 | TAK      | Opis szczegółowy poniżej                                  |
+------------------+-----------------------------------------------------------------------+----------+-----------------------------------------------------------+

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