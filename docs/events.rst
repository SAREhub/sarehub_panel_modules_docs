.. _events_ref:

###############
Obsługa zdarzeń
###############

System pozwala na określenie akcji jaka ma być wykonana przy określonych zdarzeniach systemu.

Definicja obsługi zdarzeń składa się listy zdarzeń, gdzie kluczem jest nazwa zdarzenia,
a wartością jest obiekt składający się z dodatkowej konfiguracji - **config**
oraz listy funkcji do wykonania podczas wywołania zdarzenia - **listeners**.

Przykład:

.. code-block:: json

  {
    "events": {
      "entity.changed": {
        "config": {},
        "listeners": ["..."]
      },
      "campaign.saved": {
        "config": {},
        "listeners": ["..."]
      }
    }
  }
 
Defincja funkcji składa się z pola **type** oznaczającą nazwę funkcji oraz dodatkowej konfiguracji - **config**.

Przykład:

.. code-block:: json

  {
    "events": {
      "entity.changed": {
        "config": {},
        "listeners": [
          {
            "type": "findEntityInCampaign",
            "config" : {
              "...": "..."
            }
          }
        ]
    }
  }

Możliwa jest również definicja składająca się tylko z nazwy, jeśli w określonym przypadku nie jest wymagana konfiguracja(np. gdy poprzednia funkcja ustawi odpowiednie parametry):

Przykład:

.. code-block:: json

  {
    "events": {
      "entity.changed": {
        "config": {},
        "listeners": [
          "reloadCampaigns"
        ]
      }
    }
  }
 
Funkcje są uruchamiana zgodnie z kolejnością z jaką zostały zdefiniowane - z góry na dół. Funkcje mogą zapisywać parametry do wykorzystania w następnej funkcji.
Konfiguracje funkcji są wstępnie 

Dostępne zdarzenia
******************

Zmiana encji systemowej - entity.changed
========================================

Zdarzenie wyłowywane w momencie zapisu określonej encji systemowej - szablonu oraz zgodę web-push, personalizacji, treść statyczna oraz grupy testowej.

Zdarzenie posiada następujące właściwości:

+----------+------------------------+--------------------------------------------------------------------------------------------------------------------+
| Parametr |          Opis          |                                                       Uwagi                                                        |
+==========+========================+====================================================================================================================+
| type     | typ zapisanej encji    | personalizationTemplate, productFeed, testGroup, staticContent, notificationsTemplate, notificationsPromptTemplate |
+----------+------------------------+--------------------------------------------------------------------------------------------------------------------+
| entity   | obiekt zapisanej encji |                                                                                                                    |
+----------+------------------------+--------------------------------------------------------------------------------------------------------------------+

Pomyślne zintegrowanie systemu - integration.completed
======================================================

Zdarzenie wyłowywane w momencie gdy użytkownik pomyślne zakończy proces integracji.

Zdarzenie posiada następujące właściwości:

+---------------+----------------------------------------+-------+
|   Parametr    |                  Opis                  | Uwagi |
+===============+========================================+=======+
| system        | obiekt zintegrowanego systemu          |       |
+---------------+----------------------------------------+-------+
| systemAccount | obiekt zintegrowanego konta w systemie |       |
+---------------+----------------------------------------+-------+

Pomyślne usunięcie zintegrowanego systemu - integration.deleted
===============================================================

Zdarzenie wyłowywane w momencie gdy użytkownik pomyślne usunie integrację z konta

Zdarzenie posiada następujące właściwości:

+---------------+----------------------------------------+-------+
|   Parametr    |                  Opis                  | Uwagi |
+===============+========================================+=======+
| system        | obiekt zintegrowanego systemu          |       |
+---------------+----------------------------------------+-------+
| systemAccount | obiekt zintegrowanego konta w systemie |       |
+---------------+----------------------------------------+-------+

Uruchamianie kampanii - campaign.starting
=========================================

Zdarzenie wywoływane podczas uruchamiania kampanii.

Zdarzenie posiada następujące właściwości:

+----------+----------------------+-------+
| Parametr |         Opis         | Uwagi |
+==========+======================+=======+
| account  | obiekt konta systemu |       |
+----------+----------------------+-------+
| campaign | obiekt kampanii      |       |
+----------+----------------------+-------+

Uruchamianie bloczka kampanii - campaign.node.starting
======================================================

Podobne jak *campaign.starting* tylko wywołane dla każdego bloczka w kampanii

+----------+------------------------------+-------+
| Parametr |             Opis             | Uwagi |
+==========+==============================+=======+
| block    | obiekt uruchamianego bloczka |       |
+----------+------------------------------+-------+
| campaign | obiekt kampanii              |       |
+----------+------------------------------+-------+
| session  | obiekt konta systemu         |       |
+----------+------------------------------+-------+

Uruchomiona kampania - campaign.started
=======================================

Zdarzenie wywoływane przed zakończeniem procesu uruchamiana kampanii.

Zdarzenie posiada następujące właściwości:

+----------+----------------------+-------+
| Parametr |         Opis         | Uwagi |
+==========+======================+=======+
| account  | obiekt konta systemu |       |
+----------+----------------------+-------+
| campaign | obiekt kampanii      |       |
+----------+----------------------+-------+

Uruchamianie bloczka kampanii - campaign.node.started
=====================================================

Podobne jak *campaign.started* tylko wywołane dla każdego bloczka w kampanii

+----------+------------------------------+-------+
| Parametr |             Opis             | Uwagi |
+==========+==============================+=======+
| block    | obiekt uruchamianego bloczka |       |
+----------+------------------------------+-------+
| campaign | obiekt kampanii              |       |
+----------+------------------------------+-------+
| session  | obiekt konta systemu         |       |
+----------+------------------------------+-------+


Dostępne funkcję obsługi zdarzeń
********************************

Wyszukiwanie encji w kampanii - findEntityInCampaign
====================================================

Funkcja pozwala na wyszukanie kampanii w których użyta jest dana encja.

Konfiguracja:

+-----------+-------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------------------+
| Parametr  |                                                       Opis                                                        | Wymagane |                     Uwagi                     |
+===========+===================================================================================================================+==========+===============================================+
| campaigns | Pozwala na ograniczenie kampanii do wyszukania                                                                    | NIE      |                                               |
+-----------+-------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------------------+
| entityId  | Lista gdzie kluczem jest identyfikator pola ustawień bloczka, a wartością jest właściwość encji powiązana z polem | TAK      | identyfikatory grup testowych - test_group_id |
+-----------+-------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------------------+


Parametry ograniczenia wyszukiwania kampanii - **campaigns**

+---------------+-----------------------------------------+----------+-----------------------------------------+
|   Parametr    |                  Opis                   | Wymagane |                  Uwagi                  |
+===============+=========================================+==========+=========================================+
| functionality | Funkcjunalność do wyszukania w kampanii | NIE      |                                         |
+---------------+-----------------------------------------+----------+-----------------------------------------+
| status        | Status kampanii                         | NIE      | active - aktywne, inactive - wstrzymane |
+---------------+-----------------------------------------+----------+-----------------------------------------+

Przykład z użyciem zdarzenia entity.changed:

.. code-block:: json

  {
    "events": {
      "entity.changed": {
        "config": {},
        "listeners": [
          {
            "type": "findEntityInCampaign",
            "config": {
              "campaigns": {
                "functionality": [
                  "email",
                  "database",
                  "mobile"
                ],
                "status": "active"
              },
              "entityId": {
                "@anyOf": {
                  "items": [
                    {
                      "if": "event.getType() = 'testGroup'",
                      "content": {
                        "test_group_id": "{{event.getEntity().getId()}}"
                      }
                    },
                    {
                      "if": "event.getType() = 'personalizationTemplate'",
                      "content": {
                        "personalization_template_id": "{{event.getEntity().getId()}}",
                        "data_source_cart": "{{event.getEntity().getId()}}",
                        "data_source_recommendation": "{{event.getEntity().getId()}}"
                      }
                    }
                  ],
                "options": {
                  "merge": true
                }
              }
              }
            }
          }
        ]
      }
    }
  }

Funkcja zapisuje wyszukanie identyfikatory w parametrze campaigns w właściwości id.


Ponowne uruchomienie kampanii - reloadCampaigns
===============================================

Funkcja pozwala na przeładowanie kampanii w koncie.

Konfiguracja:

+-----------+------------------------------------------------+----------+-------+
| Parametr  |                      Opis                      | Wymagane | Uwagi |
+===========+================================================+==========+=======+
| campaigns | Pozwala na ograniczenie kampanii do wyszukania | TAK      |       |
+-----------+------------------------------------------------+----------+-------+

Parametry ograniczenia wyszukiwania kampanii - **campaigns**

+----------+-----------------+----------+-----------------------------------------+
| Parametr |      Opis       | Wymagane |                  Uwagi                  |
+==========+=================+==========+=========================================+
| id       | Id kampanii     | NIE      |                                         |
+----------+-----------------+----------+-----------------------------------------+
| status   | Status kampanii | NIE      | active - aktywne, inactive - wstrzymane |
+----------+-----------------+----------+-----------------------------------------+
| type     | Typ kampanii    | NIE      |                                         |
+----------+-----------------+----------+-----------------------------------------+

Przykład:

.. code-block:: json

  {
    "events": {
      "entity.changed": {
        "config": {},
        "listeners": [
          {
            "type": "reloadCampaigns",
            "config": {
              "campaigns": {
                "id": "{{params['campaigns']['id']}}"
              }
            }
          }
        ]
      }
    }
  }

Jeśli poprzednia funkcja zapisała parametr campaigns (np. findEntityInCampaign), funkcję **reloadCampaigns** można zapisać bez konfiguracji

Przykład:

.. code-block:: json

  {
    "events": {
    "entity.changed": {
      "config": {},
      "listeners": [
        {
          "type": "findEntityInCampaign",
          "config": {
            "campaigns": {
              "functionality": [
                "email",
                "database",
                "mobile"
              ],
              "status": "active"
            },
            "entityId": {
              "@anyOf": {
                "items": [
                  {
                    "if": "event.getType() = 'testGroup'",
                    "content": {
                      "test_group_id": "{{event.getEntity().getId()}}"
                    }
                  },
                ],
                "options": {
                  "merge": true
                }
              }
            }
          }
        },
        "reloadCampaigns"
      ]
    }
  }

Wywołanie API - api-call
========================

Funkcja służy do wywołania zapytania do API

Konfiguracja:

+----------+----------------------+----------+-------+
| Parametr |         Opis         | Wymagane | Uwagi |
+==========+======================+==========+=======+
| client   | Klient API Guzzle    | TAK      |       |
+----------+----------------------+----------+-------+
| request  | Konfiguracja żądania | TAK      |       |
+----------+----------------------+----------+-------+

Konfiguracja żądania:

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
    "events": {
      "campaign.starting": {
        "config": {},
        "listeners": [
          {
            "type": "api-call",
            "config": {
              "client": "test_api",
              "request": {
                "method": "PUT",
                "url": "hubs/{{event.getAccount().id}}/campaigns/{{event.getCampaign().getId()}}/test",
                "options": {
                  "json": {
                    "test": 1
                  }
                }
              }
            }
          }
        ]
      }
    }
  }
