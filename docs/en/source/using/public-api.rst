Public API
##############

The Public API is the gateway to Dear Diary. It offers several *endpoints* 
(access points) through which different data can be requested from databases
data.

Learn how to use the API however you prefer, whether by watching a recording of a 
usage workshop or through a textual tutorial, below.

- **Acess**: https://queridodiario.ok.org.br/api/docs

.. warning::
    **We do not impose any restrictions on API consumption**. We hope for common sense 
    of developers to keep the request rate low, avoiding 
    harm our *server* and the stability of access to Dear Diary data.

    Reference: 60 requests/minute.

API Usage Workshop
************************

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

When accessing the `API page`_, there are two blocks: *default* and *schemas*.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/api-docs.png

**Schemas** describe objects and their details. Through them, you can get to know
the data that Dear Diary stores and its types.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/api-gazetteitem.png

In **default**, there are *endpoints* which are the access routes to data. To 
try one of them, select it and click the "Try it out" button, so
*endpoint* fields will be available for interaction.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/api-tryitout.png

For this experiment, we search for "budget" in the official journals 
of the municipality of Batalha (AL) through the *endpoint* ``/gazettes``. When clicking to 
execute, an access is made to the Public API and a *response* is displayed.

.. important::
    We take an *endpoint* as an example, however all other *endpoints* are 
    simulable with the same mechanics.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/api-response.png

In the **response body** field you can check the search results: how many 
files serve the search, a small excerpt from the text of each diary 
official where the keyword is found, information about it, such as date, source
of the file in the Dear Diary storage cloud, the edition number, etc.

However, this information can also be obtained via :doc:`search-interface` 
with much more user-friendly navigation. Here, what is of interest, in fact, is the URL
generated in the **Response URL** field. Our example search generates the following URL. Try it 
Click and see what it offers:

https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706&querystring=or%C3%A7amento&excerpt_size=500&number_of_excerpts=1&size=10

This URL is a permanent *link* to the search with specific settings. She 
can be added to *scripts*, such as data analysis code, to make 
automated use of Dear Diary data.

Understanding the Response URL
---------------------------

Navigate through the API page, trying different *endpoints*, configured 
with different fields is an initial step that allows greater familiarity with 
how Dear Diary data is accessed and understanding patterns of 
filling. Continuing with the example above, the generated URL explains the fields: 

... the API was accessed: ``https://queridodiario.ok.org.br/api/``

... in the "gazettes" access point: ``https://queridodiario.ok.org.br/api/gazettes?``

... searching for IBGE code territory "2700706": ``https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706``

... using the *search string* "budget": ``https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706&querystring=or%C3%A7amento``

... and so on.

As you get used to the URL construction standards, this step of using the page
of the API to generate a fixed URL, created for a very specific search, will not be 
most necessary. It is possible to start from a generalization and automate the filling
of the fields, creating custom URLs at *script* runtime. 

An example, using the *endpoint* ``gazettes/``, highlighting the places where the 
*script* would fill in the fields:

``https://queridodiario.ok.org.br/api/gazettes?territory_ids={**CÓDIGO IBGE**}querystring={**PALAVRAS-CHAVE**}&excerpt_size={**VALOR**}&number_of_excerpts={**VALOR**}&size={**VALOR**}``

.. REFERÊNCIAS
.. _página da API: https://queridodiario.ok.org.br/api/docs
.. _Python: https://www.python.org/
.. _FastAPI: https://fastapi.tiangolo.com/
.. _Swagger: https://swagger.io/