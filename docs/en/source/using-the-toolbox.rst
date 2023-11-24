Using the Toolbox
========================

|python_version_img|
|pypi_img|

The objective of `querido-diario-toolbox`_ is to equip Querido Diário's (QD) 
community with the tools necessary to conduct their own analyses and data 
manipulations using data obtained through QD. Additionally, the library 
will be integrated into the production applications used by Querido Diário, 
meaning that anyone using the library will be able to locally reproduce the 
same data processing steps performed by QD.

The library provides many levels of abstractions to work with the data. 
Ranging from simple text cleaning using strings to converting files from 
various formats into plain text.

Installing
----------

.. code-block:: python

    pip install querido-diario-toolbox

Currently, `querido-diario-toolbox` is compatible with Python 3.8+.

To perform the text extractions it's necessary to install `Tesseract OCR`_, as 
well as the ``.jar`` files from `Apache Tika`_ (last tested version: 1.24.1) and 
`Tabula`_ (last tested version: v1.0.4) accessible in order to pass their file 
paths as arguments.

Use case
---------------

More elaborate examples are available in the `examples`_ folder. You can view 
them (and interact if you wish) using `Jupyter`_ notebooks.

Removing unnecessary spaces in text
............................................

.. code-block:: python

    from querido_diario_toolbox.process.text_process import remove_breaks

    texto = "\n\n\nThis text has many      white spaces\n\n \nunnecessary.\n"

    remove_breaks(texto)
    'This text has many white spaces unnecessary.'

Finding valid `CNPJs`_ in text
.....................................

.. code-block:: python

    from querido_diario_toolbox.process.edition_process import extract_and_validate_cnpj
    
    texto = "The companies with valid CNPJs 00.000.000/0001-91 and 00.360.305/0001-04 exist, but the one with CNPJ 12.123.123/1234.12 does not exist..."
    
    extract_and_validate_cnpj(texto)
    ['00.000.000/0001-91', '00.360.305/0001-04']

Converting file from closed format to plain text and extracting metadata
............................................................................

.. code-block:: python

    from querido_diario_toolbox import Gazette
    from querido_diario_toolbox.etl.text_extractor import create_text_extractor

    config = {"apache_tika_jar": "caminho/apache/tika/jar/tika-app-1.24.1.jar"}
    extrator = create_text_extractor(config)

    diario = Gazette(filepath="caminho/diario/fechado/diario.pdf")

    extrator.extract_text(diario)
    extrator.extract_metadata(diario)
    extrator.load_content(diario)

After the execution of ``extrator.load_content(diario)``, two files (a ``.txt`` 
with pure text and a ``.json`` with metadata) will be created.

.. |python_version_img| image:: https://img.shields.io/pypi/pyversions/querido-diario-toolbox
                        :target: https://pypi.org/project/querido-diario-toolbox/
.. |pypi_img| image:: https://img.shields.io/pypi/v/querido-diario-toolbox
              :target: https://pypi.org/project/querido-diario-toolbox/
.. _querido-diario-toolbox: https://pypi.org/project/querido-diario-toolbox/
.. _Tesseract OCR: https://tesseract-ocr.github.io/tessdoc/
.. _Apache Tika: https://tika.apache.org/download.html
.. _examples: https://github.com/okfn-brasil/querido-diario-toolbox/tree/main/examples
.. _Tabula: https://github.com/tabulapdf/tabula-java/releases
.. _Jupyter: https://jupyter.org/
.. _CNPJs: https://en.wikipedia.org/wiki/CNPJ
