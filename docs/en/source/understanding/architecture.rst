Architecture
======================

The project relies on various systems to ensure that the daily publications 
from the publisher's website arrive here in an open way and make interaction 
with this database accessible. Below are items responsible for each part of the project.

Data Processing
----------------------

This stage is essential because it's where the magic happens to transform closed data 
into open data and connect other parts of our data flow. `In the repository`_ you'll 
find various technologies such as `Apache Tika`_ for extracting text from documents in 
various formats, `elasticsearch`_ for interacting with the text indexer of the documents, 
`postgresql`_ for interacting with the data extracted from the spiders, and a lot of pure Python.

Data Collection
---------------

To understand Querido Diário project, it's crucial to know how the structure of 
collecting official gazettes files works. All scripts are developed using the Scrapy framework, a very popular tool for web scraping. 
If you've never used it before, the tutorial is a great way to learn how to 
create your first data collection scripts. We have a dedicated repository for 
organizing the collection of files. In it, you'll find Python files developed by 
the community for each municipality, and we call these files spiders 
(Spider is the term used for a data scraping script in Scrapy).

Toolbox
-------

The goal of the `Querido Diário Toolbox`_ is to provide the community with a set 
of tools that enable the analysis and manipulation of data extracted from official gazettes. 
With the toolbox, we expand the possibilities for new projects that use the database and share 
the knowledge accumulated in cleaning and processing the data that the project has already faced. 
The toolbox is also used internally by the OKBR team for processing the gazettes offered in the API; 
it allows for data extraction, metadata handling, removal of irrelevant characters, and extraction of CNPJ (Brazilian identification numbers). The toolbox is not, and will never be, a completed project: 
with the support of the community, we will continually add new tools and make them available to anyone 
interested in the project!

API
-----------

The `public API`_ is one of the project's main layers because it allows the 
dissemination of the database in an accessible and automatable by machines, 
as well as providing direct access to the data for developers and researchers. Additionally, 
the platform consumes data from the API to make it available to the general public.

The API was built in Python with `FastAPI`_ and is integrated with Swagger 
for automatic documentation generation. You can contribute with ideas in `API's repository`_.

Frontend 
---------

This search platform is a visual interface for interacting with the public API and also a 
tool for disseminating knowledge and encouraging the use of official gazettes.

Its development is primarily done in Angular with TypeScript, with mobile responsiveness as 
a top priority. If you're interested in contributing, the `repository`_ is also open.


.. _In the repository: https://github.com/okfn-brasil/querido-diario-data-processing/
.. _Apache Tika: https://tika.apache.org/
.. _elasticsearch: https://www.elastic.co/
.. _postgresql: https://www.postgresql.org/ 
.. _Querido Diário Toolbox: https://github.com/okfn-brasil/querido-diario-toolbox
.. _public API: https://queridodiario.ok.org.br/api/docs
.. _FastAPI: https://fastapi.tiangolo.com/
.. _API's repository: https://github.com/okfn-brasil/querido-diario-api
.. _repository: https://github.com/okfn-brasil/querido-diario-frontend
