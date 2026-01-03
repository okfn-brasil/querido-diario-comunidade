Architecture
############

Querido Diário deals with different challenges -- described in :ref:`Facing the data
desert` -- and employs various resources to overcome each of them, in order to
fulfill the :ref:`Pillars<Pillars: the values of Querido Diário>` of the project.

Its architecture is, therefore, a reflection of the technical decisions that
connect these two ends: the set of solutions necessary to face the obstacles
imposed by the availability of official gazettes and the interest in opening
this data.

We can summarize the flow of the project for each type of official gazette in
the steps below and understand them in more detail later.

1. **Collect**: obtain official gazette files from the source, the publishing
   websites
2. **Process**: apply treatments to the original files obtained
3. **Provide**: allow access and search on the stored contents

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-dataflow-per-gazettes.png
   :alt: [TODO]

1. Data collection
******************

Here, scraper robots are created to periodically collect official gazette files
and associated metadata, such as publication date and edition number, from the
publishing websites. Regardless of the type of official gazette, all of them
require a scraper for file collection.

- **Development**: `Python`_ using the `Scrapy`_ framework.
- **Production storage**: DO Spaces for files and `PostgreSQL`_ for metadata.
- **Code repository**: https://github.com/okfn-brasil/querido-diario
- **Collection scheduling**:
    - Historical series available on the website: once, when the municipality
      is integrated into the project.
    - Most recent edition: every day at 6 p.m., Brasília time.
    - Previous month (redundancy): every 1st day of the month at 8 p.m., Brasília
      time.
- **Learn more**: :doc:`Documentation on Scrapers<../contributing/scrapers>`

2. Data processing
******************

This is the heart of Querido Diário, the stage responsible for text operations
and for connecting the other components of the architecture.

Here, plain text is extracted from the official gazettes, saved in open TXT format
along with the original file, and its content and metadata are indexed in the
search engine to enable queries on the texts.

.. note::
    Even though Text PDFs are the most common format provided by municipalities,
    this stage can also extract text from DOC and DOCX files.

This is also the stage where the integration of aggregated gazettes is implemented
via segmenters. These are tools that slice the portions related to each municipality,
from the various ones present in the file’s content, and produce individual TXT
files for each municipality with the corresponding excerpts.

Finally, thematic filtering is performed, where searches are made in the primary
index of the gazettes, to then create indexes with excerpts associated with specific
themes like "Technologies in Education" and "Environmental Policies".

.. warning::
    Currently, the project does not integrate cases where the file format is Image
    PDF due to the low quality of text extraction from images (OCR). Similarly,
    fragmented gazettes are not integrated due to database modeling limitations.

- **Development**: `Python`_, `Apache Tika`_ for text extraction and `OpenSearch`_ for
  thematic filters.
- **Production storage**: DO Spaces for files, `PostgreSQL`_ for metadata, and
  `OpenSearch`_ as the search engine.
- **Code repository**: https://github.com/okfn-brasil/querido-diario-data-processing

3. Data providing
*****************

API
===

The Public API is the gateway through which access to the project’s data is
facilitated. It has several endpoints through which different
data can be requested from the official gazette databases and
`CNPJs from the federal taxes service`_ (*Receita Federal do Brasil*, in Portuguese).

- **Access**: https://api.queridodiario.ok.org.br/docs
- **Development**: `Python`_ using the `FastAPI`_ framework and automatic documentation
  with `Swagger`_.
- **Production storage**: `PostgreSQL`_ for company metadata and `OpenSearch`_ for gazette
  searches.
- **Code repository**: https://github.com/okfn-brasil/querido-diario-api
- **Learn more**: :doc:`Public API Resources<../using/public-api>`

Backend
=======

The backend is responsible for being the intermediary mechanism between the
Querido Diário frontend and its Public API when necessary. As users navigate the
site, some of their actions trigger this mechanism to request data from the
Public API, perform operations, and deliver the results to the site.

- **Development**: `Python`_, `Django REST Framework`_ and asynchronous task management
  with `Celery`_.
- **Production storage**: `PostgreSQL`_ for user and alert management and `Redis`_ for
  asynchronous tasks.
- **Code repository**: https://github.com/okfn-brasil/querido-diario-backend

Web interface
=============

This is the official Querido Diário's website, its visual interface for users via
Internet navigation. It contains the search interface for official gazettes. The
site also serves as a dissemination tool for the project: it records its history
and the people who are part of it, as well as producing knowledge around it, such
as analyses, reports, and research.

- **Access**: https://queridodiario.ok.org.br
- **Development**: `TypeScript`_ and `Angular`_ framework
- **Code repository**: https://github.com/okfn-brasil/querido-diario-frontend
- **Learn more**: :doc:`Search Interface Resources<../using/search-interface>`

Support
*******

The official technical documentation of the project is meant to help the understanding
of interested people, users, and developers, recording essential and more deeply
described information on how Querido Diário works.

As it also serves as a "conversation between the project and its contributors," the
documentation repository contains the project’s roadmap (:ref:`Roadmap`).

- **Development**: `Sphinx`_
- **Code repository**: https://github.com/okfn-brasil/comunidade
- **Learn more**: :doc:`Documentation on Documentation<../contributing/documentation>`

Architecture design
*******************

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-architecture.png
   :alt: [TODO]

.. References
.. _Python: https://www.python.org/
.. _Scrapy: https://scrapy.org/
.. _Apache Tika: https://tika.apache.org/
.. _OpenSearch: https://opensearch.org/
.. _PostgreSQL: https://www.postgresql.org/
.. _FastAPI: https://fastapi.tiangolo.com/
.. _Pydantic: https://docs.pydantic.dev/
.. _Swagger: https://swagger.io/
.. _Django REST Framework: https://www.django-rest-framework.org/
.. _TypeScript: https://www.typescriptlang.org/
.. _Angular: https://angular.io/
.. _Sphinx: https://www.sphinx-doc.org/pt-br/master/
.. _Celery: https://docs.celeryq.dev/en/stable/
.. _Redis: https://redis.io/
.. _CNPJs from the federal taxes service: https://github.com/okfn-brasil/receita
