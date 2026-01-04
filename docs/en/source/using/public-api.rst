Public API
##############

The Public API is the gateway to Querido Diário. It offers several endpoints 
(access points) through which different data can be requested from databases.

Learn how to use the API however you prefer, whether by watching a recording of a 
usage workshop or through a textual tutorial, below.

- **Documentation**: https://api.queridodiario.ok.org.br/docs

.. warning::
    **We do not impose any restrictions on API usage**. We hope for common sense 
    of developers to keep the request rate low, avoiding harm our server and the 
    stability of access to Querido Diário data.

    Reference: 60 requests/minute.

API Usage Workshop (in Portuguese)
**************************************

.. raw:: html

    <div style="position:relative;padding-bottom:56.25%;margin-bottom:3vh">
        <iframe style="width:100%;height:100%;position:absolute;left:0px;top:0px;"
        src="https://www.youtube.com/embed/vgrqw2HLFJY?si=YMZ3WdnaVVBPpWdr" 
        title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; 
        encrypted-media; gyroscope; picture-in-picture; web-share" 
        allowfullscreen></iframe>
    </div>
                                                                                
API usage tutorial
******************************

When accessing the `API docs page`_, there are two blocks: *default* and *schemas*.

.. image:: https://d20vl87kcc4i28.cloudfront.net/docs/api-docs.png

**Schemas** describe objects and their details. Through them, you can get to know
the data that Querido Diário stores and its types.

.. image:: https://d20vl87kcc4i28.cloudfront.net/docs/api-gazetteitem.png

In **default**, there are endpoints which are the access routes to data. To 
try one of them, select it and click the "Try it out" button, so
endpoint fields will be available for interaction.

.. image:: https://d20vl87kcc4i28.cloudfront.net/docs/api-tryitout.png

For this experiment, we search for "orçamento" (budget) in the official gazettes 
of Batalha (AL) municipality (ID 2700706) through the endpoint ``/gazettes``. When 
clicking to execute, an access is made to the Public API and a response is displayed.

.. important::
    We take an endpoint as an example, however all other endpoints are 
    simulable with the same mechanics.

.. image:: https://d20vl87kcc4i28.cloudfront.net/docs/api-response.png

In the **response body** field you can check the search results: how many 
files serve the search, a small excerpt from each official gazette where the 
keyword is found, and information about it, such as date, source
of the file in the Querido Diário storage cloud, the edition number, etc.

However, this information can also be obtained via :doc:`search-interface` 
with much more user-friendly navigation. Here, what is of interest, in fact, is the URL
generated in the **Response URL** field. Our example search generates the following URL. 
Try clicking in it and see what it offers:

https://api.queridodiario.ok.org.br/gazettes?territory_ids=2700706&querystring=or%C3%A7amento&excerpt_size=500&number_of_excerpts=1&size=10

This URL is a permanent link to the search with this specific settings. It can be 
added to scripts, such as data analysis code, to make automated use of Querido 
Diário data.

Understanding the Response URL
-----------------------------------

Navigate through the API page, trying different endpoints, configured 
with different fields is an initial step that allows greater familiarity with 
how Querido Diário data is accessed. Continuing with the example above, the generated 
URL explains the fields: 

... the API was accessed: ``https://api.queridodiario.ok.org.br/``

... in the "gazettes" endpoint: ``https://api.queridodiario.ok.org.br/gazettes?``

... searching for IBGE code territory "2700706": ``https://api.queridodiario.ok.org.br/gazettes?territory_ids=2700706``

... using the search string "orçamento": ``https://api.queridodiario.ok.org.br/gazettes?territory_ids=2700706&querystring=orçamento``

... and so on.

As you get used to the URL construction pattern, this step of using the API page 
to generate a fixed URL, created for a very specific search, will not be necessary. 
It is possible to start from a generalization and automate the filling
of the fields, creating custom URLs at script runtime. 

An example, using the endpoint ``gazettes/``, highlighting the places where the 
script would fill in the fields:

``https://api.queridodiario.ok.org.br/gazettes?territory_ids={**ID**}querystring={**SEARCH KEYWORDS**}&excerpt_size={**VALUE**}&number_of_excerpts={**VALUE**}&size={**VALUE**}``

.. REFERÊNCIAS
.. _API docs page: https://api.queridodiario.ok.org.br/docs
.. _Python: https://www.python.org/
.. _FastAPI: https://fastapi.tiangolo.com/
.. _Swagger: https://swagger.io/