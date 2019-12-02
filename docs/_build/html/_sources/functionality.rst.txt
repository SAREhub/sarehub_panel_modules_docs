.. _functionality_ref:

##############
Funkcjonalność
##############

Każdy moduł MOŻE zawierać jedną lub kilka funkcjonalności.
Funkcjonalność stanowi najbardziej podstawą cześć modułu systemu i składa sie głównie z "bloczka" w schemacie kampanii i formularza.

+----------+------------------------------------------------+----------+-------+
| Parametr |                      Opis                      | Wymagane | Uwagi |
+==========+================================================+==========+=======+
| label    | Nazwa funkcjonalności wyświetlana w systemie   | TAK      |       |
+----------+------------------------------------------------+----------+-------+
| config   | Konfiguracja funkcjonalności                   | TAK      |       |
+----------+------------------------------------------------+----------+-------+
| node     | Konfiguracja bloczka wyświetlanego w schemacie | TAK      |       |
+----------+------------------------------------------------+----------+-------+
| form     | Konfiguracja formularza bloczka                | TAK      |       |
+----------+------------------------------------------------+----------+-------+


Definicja bloczka - node
************************

+-----------+---------------------------------+----------+----------------------------------------------------------+
| Parametr  |              Opis               | Wymagane |                          Uwagi                           |
+===========+=================================+==========+==========================================================+
| icon      | Ikona wyświetlana na bloczku    | NIE      |                                                          |
+-----------+---------------------------------+----------+----------------------------------------------------------+
| tab       | Kategoria bloczka               | TAK      | source - trigger, analitics - filtry, executable - akcje |
+-----------+---------------------------------+----------+----------------------------------------------------------+
| endpoints | Lista wejść i wyjść bloczka     | TAK      |                                                          |
+-----------+---------------------------------+----------+----------------------------------------------------------+


Lista wejść i wyjść - endpoints
===============================


Definicja formularza - form
***************************


Lista pól - fields
==================

Pole tekstowe - text, url, email
--------------------------------
Podstawowe pole tekstowe. Ustawiając typ url lub email, nastąpi wymuszenie poprawności wpisanej wartość dla odpowiedniego typu. 

.. image:: /fields/text.png

+-------------+-----------------------------+----------+-----------------------------------+
|  Parametr   |            Opis             | Wymagane |               Uwagi               |
+=============+=============================+==========+===================================+
| id          | Unikalny identyfikator pola | TAK      |                                   |
+-------------+-----------------------------+----------+-----------------------------------+
| label       | Etykieta pola               | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+-----------------------------+----------+-----------------------------------+
| placeholder | Teks pomocniczy             | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+-----------------------------+----------+-----------------------------------+
| required    | Wymagalność pola            | NIE      | true/false                        |
+-------------+-----------------------------+----------+-----------------------------------+
| maxlength   | Maksymalna liczba znaków    | NIE      |                                   |
+-------------+-----------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "text",
      "label": "%TRANSLATE|pole_tekstowe%",
      "placeholder": "%TRANSLATE|podpowiedz_pola%",
      "required": true,
      "maxlength": 30
    }


Pole numeryczne - number
------------------------
Pole edycyjne przyjmujące tylko liczby.

.. image:: /fields/number.png

+-------------+------------------------------------------------------+----------+-----------------------------------+
|  Parametr   |                         Opis                         | Wymagane |               Uwagi               |
+=============+======================================================+==========+===================================+
| id          | Unikalny identyfikator pola                          | TAK      |                                   |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| label       | Etykieta pola                                        | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| placeholder | Teks pomocniczy                                      | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| required    | Wymagalność pola                                     | NIE      | true/false                        |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| readonly    | Pole tylko do odczytu                                | NIE      | true/false                        |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| min         | Minimalna wartość, jaką może przyjąć pole            | NIE      |                                   |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| max         | Maksymalna wartość, jaką może przyjąć pole           | NIE      |                                   |
+-------------+------------------------------------------------------+----------+-----------------------------------+
| step        | Określa, o jaki zakres może zmienić się wartość pola | NIE      |                                   |
+-------------+------------------------------------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "text",
      "label": "%TRANSLATE|pole_tekstowe%",
      "placeholder": "%TRANSLATE|podpowiedz_pola%",
      "required": true,
      "min": 1,
      "max": 20,
      "step": 2,
    }

Obszar tekstowy - textarea
--------------------------

Wieloliniowe pole tekstowe.

.. image:: /fields/textarea.png

+-------------+-----------------------------+----------+-----------------------------------+
|  Parametr   |            Opis             | Wymagane |               Uwagi               |
+=============+=============================+==========+===================================+
| id          | Unikalny identyfikator pola | TAK      |                                   |
+-------------+-----------------------------+----------+-----------------------------------+
| label       | Etykieta pola               | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+-----------------------------+----------+-----------------------------------+
| placeholder | Teks pomocniczy             | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+-------------+-----------------------------+----------+-----------------------------------+
| required    | Wymagalność pola            | NIE      | true/false                        |
+-------------+-----------------------------+----------+-----------------------------------+
| rows        | Maksymalna liczba wierszy   | NIE      |                                   |
+-------------+-----------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "textarea",
      "label": "%TRANSLATE|obszar_tekstowy%",
      "placeholder": "%TRANSLATE|podpowiedz_pola%",
      "required": true,
      "rows": 5
    }

Rozwijana lista - select
------------------------

.. image:: /fields/select.png

+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| Parametr |                              Opis                               | Wymagane |               Uwagi               |
+==========+=================================================================+==========+===================================+
| id       | Unikalny identyfikator pola                                     | TAK      |                                   |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| label    | Etykieta pola                                                   | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| required | Wymagalność pola                                                | NIE      | true/false                        |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| reload   | Określenie czy po zmianie wartości formularz ma się przeładować | NIE      | true/false                        |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| values   | Opcje do wyboru w polu                                          | TAK      |                                   |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| default  | Domyślnie wybrana opcja                                         | NIE      | Identyfikator opcji               |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "select",
      "label": "%TRANSLATE|rozwijana_lista%",
      "required": true,
      "default": "option2",
      "values": [
          {
            "id": "option1",
            "label": "%TRANSLATE|opcja1%"
          },
          {
            "id": "option2",
            "label": "%TRANSLATE|opcja2%"
          },
          {
            "id": "option3",
            "label": "%TRANSLATE|opcja3%"
          }
      ]
    }

Przycisk radio - radio
----------------------

Grupa przycisków radio.

.. image:: /fields/radio.png

+----------+-----------------------------+----------+-----------------------------------+
| Parametr |            Opis             | Wymagane |               Uwagi               |
+==========+=============================+==========+===================================+
| id       | Unikalny identyfikator pola | TAK      |                                   |
+----------+-----------------------------+----------+-----------------------------------+
| label    | Etykieta pola               | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+----------+-----------------------------+----------+-----------------------------------+
| required | Wymagalność pola            | NIE      | true/false                        |
+----------+-----------------------------+----------+-----------------------------------+
| values   | Opcje do wyboru w polu      | TAK      |                                   |
+----------+-----------------------------+----------+-----------------------------------+
| default  | Domyślnie wybrana opcja     | NIE      | Identyfikator opcji               |
+----------+-----------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "radio",
      "label": "%TRANSLATE|przycisk_radio%",
      "required": true,
      "values": [
          {
            "id": "option1",
            "label": "%TRANSLATE|opcja1%"
          },
          {
            "id": "option2",
            "label": "%TRANSLATE|opcja2%"
          },
          {
            "id": "option3",
            "label": "%TRANSLATE|opcja3%"
          }
      ]
    }

Przycisk wyboru - checkbox
--------------------------

Grupa przycisków wyboru.

.. image:: /fields/checkbox.png

+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| Parametr |                              Opis                               | Wymagane |               Uwagi               |
+==========+=================================================================+==========+===================================+
| id       | Unikalny identyfikator pola                                     | TAK      |                                   |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| label    | Etykieta pola                                                   | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| required | Wymagalność pola                                                | NIE      | true/false                        |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| reload   | Określenie czy po zmianie wartości formularz ma się przeładować | NIE      | true/false                        |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+
| values   | Opcje do wyboru w polu                                          | TAK      |                                   |
+----------+-----------------------------------------------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "checkbox",
      "label": "%TRANSLATE|przycisk_wyboru%",
      "required": true,
      "values": [
          {
            "id": "option1",
            "label": "%TRANSLATE|opcja1%"
          },
          {
            "id": "option2",
            "label": "%TRANSLATE|opcja2%"
          },
          {
            "id": "option3",
            "label": "%TRANSLATE|opcja3%"
          }
      ]
    }

Wybór daty - date
-----------------

Pole wyboru daty i godziny.

.. image:: /fields/date.png

+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
|   Parametr    |               Opis               | Wymagane |                                    Uwagi                                    |
+===============+==================================+==========+=============================================================================+
| id            | Unikalny identyfikator pola      | TAK      |                                                                             |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| label         | Etykieta pola                    | TAK      | Możliwe tłumaczenia (%TRANSLATE%)                                           |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| placeholder   | Tekst pomocniczy                 | TAK      | Możliwe tłumaczenia (%TRANSLATE%)                                           |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| required      | Wymagalność pola                 | NIE      | true/false                                                                  |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| format        | Format daty/godziny do wpisania  | TAK      | - d(dzień bez zero), dd(dzień z zero) np.: 5, 05                            |
|               |                                  |          | - D(krótki dzień tygodnia), DD(pełen dzień tygodnia) np.: Pon, Poniedziałek |
|               |                                  |          | - m(miesiąc bez zero),  mm(miesiąc z zero) np.: 7, 07                       |
|               |                                  |          | - M(Krótka nazwa miesiąca), MM(Pełna nazwa miesiąca) np.: Mar, Marzec       |
|               |                                  |          | - yy(2 cyfrowy rok), yyyy(4 cyfrowy rok) np.: 12, 2012                      |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| min_view      | Minimalny widok do wybory        | NIE      | - 0, days, month - Widok wyboru dnia miesiąca                               |
|               |                                  |          | - 1, months, year - Widok wyboru miesiąca                                   |
|               |                                  |          | - 2, years, decade - Widok wyboru roku(widok dekady)                        |
|               |                                  |          | - 3, decades, century - Widok wyboru dekady(widok stulecia)                 |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| max_view      | Maksymalny widok do wyboru       | NIE      | j.w.                                                                        |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| min_date      | Minimalna możliwa data do wyboru | NIE      | "now" dla obecnej daty i godziny                                            |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+
| output_format | Format wynikowy                  | NIE      | Domyślnie X, Opcję zgodne z Moment_.                                        |
+---------------+----------------------------------+----------+-----------------------------------------------------------------------------+

.. _Moment: https://momentjs.com/docs/#/displaying/

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "date",
      "label": "%TRANSLATE|data%",
      "placeholder": "%TRANSLATE|podaj_date_i_godzine%",
      "label": "%TRANSLATE|data%",
      "format": "DD-MM-YYYY HH:mm",
      "min_date": "now",
      "max_view": "year",
      "required": true
    }


Wybór dnia tygodnia - week-picker
---------------------------------

Pole wyboru dnia tygodnia.

.. image:: /fields/week-picker.png

+----------+-----------------------------+----------+-----------------------------------+
| Parametr |            Opis             | Wymagane |               Uwagi               |
+==========+=============================+==========+===================================+
| id       | Unikalny identyfikator pola | TAK      |                                   |
+----------+-----------------------------+----------+-----------------------------------+
| label    | Etykieta pola               | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+----------+-----------------------------+----------+-----------------------------------+
| required | Wymagalność pola            | NIE      | true/false                        |
+----------+-----------------------------+----------+-----------------------------------+


Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "week-picker",
      "label": "%TRANSLATE|dzien_tygodnia%",
      "required": true
    }

Grupy pól - array
---------------------------------


.. image:: /fields/array.png

+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| Parametr  |              Opis               | Wymagane |                                               Uwagi                                                |
+===========+=================================+==========+====================================================================================================+
| id        | Unikalny identyfikator pola     | TAK      |                                                                                                    |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| label     | Etykieta pola                   | TAK      | Możliwe tłumaczenia (%TRANSLATE%)                                                                  |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| max       | Maksymalna liczba grup          | TAK      | - rule - Maksymalna liczba grup                                                                    |
|           |                                 |          | - info - Informacja która się wyświetla po osiągnięciu maksimum. Możliwe tłumaczenia (%TRANSLATE%) |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| min       | Minimalna liczba grup           | TAK      |                                                                                                    |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| fields    | Lista pól w grupie              | TAK      |                                                                                                    |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+
| addButton | Opcje przycisku dodawania grupy | TAK      | - show - Czy przycisk ma być wyświetlony                                                           |
|           |                                 |          | - disabled - Czy przycisk ma być wyłączony                                                         |
|           |                                 |          | - label - Etykieta przycisku. Możliwe tłumaczenia (%TRANSLATE%)                                    |
+-----------+---------------------------------+----------+----------------------------------------------------------------------------------------------------+

Właściwość etykiety pola(**label**) dla pól w grupie przyjmuję postać tablicy obiektów o następującej postaci:

+----------+---------------------+----------+----------------------------------------------------------------+
| Parametr |        Opis         | Wymagane |                             Uwagi                              |
+==========+=====================+==========+================================================================+
| index    | Indeks grupy        | TAK      | Numerowane od 0. Należy wstawić false dla pozostałych indeksów |
+----------+---------------------+----------+----------------------------------------------------------------+
| text     | Tresć etykiety pola | TAK      | Możliwe tłumaczenia (%TRANSLATE%)                              |
+----------+---------------------+----------+----------------------------------------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "label": "%TRANSLATE|grupa%",
      "type": "array",
      "min": 1,
      "max": {
        "rule": 5,
        "info": "%TRANSLATE|osiagnales_juz_limit_5_adresow_url%"
      },
      "init": true,
      "addButton": {
        "show": true,
        "disabled": false,
        "label": "%TRANSLATE|dodaj_strone%"
      },
      "fields": [
        {
          "id": "param2",
          "type": "text",
          "label": [
            {
              "index": 0,
              "text": "%TRANSLATE|pierwsze_pole%"
            },
            {
              "index": false,
              "text": "%TRANSLATE|pole_tekstowe%"
            }
          ],
          "placeholder": "%TRANSLATE|podpowiedz_pola%",
          "required": true,
          "maxlength": 30
        }
      ]
    }

Tekst statyczny - paragraph
---------------------------

+----------+-----------------------------+----------+-----------------------------------+
| Parametr |            Opis             | Wymagane |               Uwagi               |
+==========+=============================+==========+===================================+
| id       | Unikalny identyfikator pola | TAK      |                                   |
+----------+-----------------------------+----------+-----------------------------------+
| content  | Treść                       | TAK      | Możliwe tłumaczenia (%TRANSLATE%) |
+----------+-----------------------------+----------+-----------------------------------+
| style    | Styl elementu               | NIE      | Styl jako obiekt                  |
+----------+-----------------------------+----------+-----------------------------------+

Przykład:

.. code-block:: json

    {
      "id": "param1",
      "type": "paragraph",
      "content": "%TRANSLATE|tresc%",
      "style": {
        "margin-top": "24px"
      }
    }
