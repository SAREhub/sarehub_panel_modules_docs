.. sarehub-panel-modules documentation master file, created by
   sphinx-quickstart on Mon Mar 27 17:42:41 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.
#####
Wstęp
#####
.. toctree::
   :hidden:

   self

.. toctree::
   :maxdepth: 2

   preprocesor
   system
   module
   flowchart
   events


Wejściowym plikiem dla zdefiniowanej konfiguracji **MUSI** być plik *system.json*, który określa system z którym jest definiowana konfiguracja.

Format i struktura plików
=========================
Konfigurację **MUSI** być zdefiniowana w pliku JSON, chyba że w danym fragmencie określono inne możliwości.

Podział pliku
-------------
Mimo że całą konfigurację można zapisać w jednym pliku, zalecane jest aby kluczowe cześci konfiguracji wydzielić do różnych plików.
Odnośnik do zewnętrznego pliku należy zdefiniować pod właściwościa *@Ref*,
której wartoscią **POWINIEN** być obiekt który **MUSI** posiadać pole *path* którego wartościa **MUSI** być scieżka do określonego pliku relatywnie do głównego pliku *system.json*.
właściwość *path* oraz **MOŻE** zawierać pole *type* które określa format dołącznego pliku.

Przykład:

.. code-block:: json

    {
      "blocks": [
            {
              "@Ref" : {
                "type" : "json",
                "path" : "blocks/refBlock/refBlock.json"
              }
            }
          ]
    }

Możliwa jest również krótsza forma definicji gdzie wartoscią **MUSI** być ścieżka do dołączanego pliku relatywnie do pliku *system.json*.


Przykład:

.. code-block:: json

    {
      "blocks": [
            {
              "@Ref" : "blocks/refBlock/refBlock.json"
            }
          ]
    }
