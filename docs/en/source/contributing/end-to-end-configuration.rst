End-to-end configuration
########################

Sometimes it is necessary to integrate various stages of Querido Diário, especially
when developing frontend solutions that require data that is not yet in production.

Since QD uses `podman`_ to manage its infrastructure, orchestrating the stages
consists of adjusting environment variables and ensuring that the containers are
communicating properly.

.. attention::  
    This documentation has been primarily tested on Linux environments. Windows WSL
    or MacOS environments may have some differences (contributions are welcome!).

Starting the setup with data processing
***************************************

Since the resources (database, search engine, etc.) used by QD locally will be shared
by different stages, the `data processing`_ project is a good starting point for setup
as it uses several of these resources in its processing tasks.

To set it up, we will follow these steps:

1. `Install podman`_ (if not already installed);

2. Run:

    .. code-block:: bash

      make build

3. Run (``FULL_PROJECT=true`` allows additional ports to be opened by the pod that will be created):

    .. code-block:: bash

      FULL_PROJECT=true make setup

.. tip::  
    If this is your first time running the project, the environment variables will be
    created in the ``envvars`` file, at the root of the repository. Here you will find
    credentials for the object storage (Minio), for example.

.. warning::  
    When running the ``make setup`` command, a ``bind: address already in use`` error
    may appear. In this case, check if another service on your computer is using the
    indicated port and stop it (or modify the environment variables and/or Makefile if
    you know what you're doing).

    If the port is being used by podman itself due to an execution error, you can
    terminate the program with the command (using port 8000 as an example)
    ``sudo kill -9 $(sudo lsof -t -i:8000)``.

    After terminating, run ``make setup`` again.

Now the pod has been created, and within it, several resources like Opensearch,
Postgres, and Minio are running. However, they are still "empty." Let's populate them
using the spiders repository.

Generating data with spiders
*****************************

We want to run spiders to populate our database with official gazettes for processing.
To do this, we just need to configure the following in the `spiders repository`_:

1. Copy ``data_collection/.local.env`` to ``data_collection/.env``;

2. Set up the development environment and run spiders normally, as indicated in its
   documentation.

Processing the scraped documents
********************************

Now that we have the object storage and some Postgres database tables populated, let's
run the main data processing pipeline to generate TXT files and populate the search
engine:

1. Run in the data processing repository:

    .. code-block:: bash

      make re-run

.. note::  
    Notice that ``make re-run`` is being executed here, and not ``make run``, because
    ``make run`` executes ``make setup``, and if ``make setup`` is run, all resources
    will be destroyed and rebuilt, nullifying the scraping that was done.

Now we have files, tables, and indices populated. We can enable the API.

Enabling the API
****************

1. Run, in the `API`_ repository:

    .. code-block:: bash

      make build

2. Run:

    .. code-block:: bash

      make re-run

.. note::  
    For the same reason as in data processing, ``make re-run`` is being executed here,
    and not ``make run``.

With the API available, we can start the local backend.

Enabling the backend
********************

To handle `Querido Diário: Technologies in Education`_, the `backend`_ needs to be set
up as follows:

1. Set up the development environment as indicated in the documentation;

2. Create a superuser account as requested.

With the backend available, the frontend that uses local API and backend can also be
configured.

Enabling the frontend
*********************

Finally, we've reached the other end of the QD architecture, the `frontend`_! Here
we'll do the following:

1. Set up the development environment as indicated in the documentation;

2. Apply this patch in the repository:

    .. code-block:: bash

      ./src/app/constants.ts
      - export const API = 'https://api.queridodiario.ok.org.br';
      + export const API = 'http://localhost:8080';

      ./src/app/services/utils/index.ts
      - export const educationApi = 'https://backend-api.api.queridodiario.ok.org.br/';

      + export const educationApi = 'http://localhost:8000/api/';

Done! Now the entire environment is configured 🎉

Environment usage tips  
**********************

Some useful ways to use the development environment:

1. Want to access the postgres database to see official gazette records, backend users,
   etc.?

    Run ``make shell-database`` in the querido-diario-data-processing repository and you'll be in the ``psql`` command line.

2. Want to access the search engine to see textual indices of gazettes and thematic
   excerpts?

    Run:

    .. code-block:: bash

      curl -k -u admin:admin -X GET "localhost:9200/_cat/indices?v&pretty=true

    Other endpoints will work similarly according to the Opensearch documentation.

3. Want to access the file system to see where they were downloaded?

    Go to `localhost:9000 <http://localhost:9000>`_ with the credentials found in the
    ``envvars`` of the querido-diario-data-processing repository.

4. Want to download more gazette files and process them?

    Run another ``scrapy crawl`` in the querido-diario repository and then run
    ``make re-run`` in querido-diario-data-processing again.

5. In the frontend, live reload is enabled, but not in the API and backend. How to check changes?

    In the API, run ``make re-run`` again. In the backend, run:

    .. code-block:: bash

      python -m cli setup -- pod-name querido-diario

6. How to access the API documentation?

    Go to `0.0.0.0:8080/docs <0.0.0.0:8080/docs>`_.

    .. tip::  
        If ``0.0.0.0`` doesn't work, use `localhost:8080/docs <http://localhost:8080/docs>`_.

7. How to access the backend admin panel?

    Go to `0.0.0.0:8000/api/admin <http://0.0.0.0:8000/api/admin>`_ with the superuser
    credentials created earlier.

    .. tip::  
        If ``0.0.0.0`` doesn't work, use `localhost:8000/api/admin <http://localhost:8000/api/admin>`_.

.. References  
.. _podman: https://podman.io/  
.. _data processing: https://github.com/okfn-brasil/querido-diario-data-processing/  
.. _spiders repository: https://github.com/okfn-brasil/querido-diario/
.. _API: https://github.com/okfn-brasil/querido-diario-api/
.. _backend: https://github.com/okfn-brasil/querido-diario-backend/
.. _frontend: https://github.com/okfn-brasil/querido-diario-frontend/
.. _Install podman: https://podman.io/docs/installation  
.. _Querido Diário\: Technologies in Education: https://queridodiario.ok.org.br/educacao
