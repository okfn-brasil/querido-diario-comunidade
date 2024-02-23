API Pública
##############

A API Pública é a porta de acesso ao Querido Diário. Ela oferece diversos *endpoints* 
(pontos de acesso) pelos quais diferentes dados podem ser solicitados às bases de
dados.

Aprenda como utilizar a API conforme preferir, seja assistindo a gravação de uma 
oficina de uso ou por meio de tutorial textual, logo abaixo.

- **Acesse**: https://queridodiario.ok.org.br/api/docs

.. warning::
    **Não impomos nenhuma restrição para consumo da API**. Esperamos bom senso por parte 
    de pessoas desenvolvedoras para manter taxa de requisição baixa, evitando 
    prejudicar nosso *servidor* e a estabilidade do acesso aos dados do Querido Diário.

    Referência: 60 requisições/minuto.

Oficina de uso da API
************************

.. raw:: html

    <div style="position:relative;padding-bottom:56.25%;margin-bottom:3vh">
        <iframe style="width:100%;height:100%;position:absolute;left:0px;top:0px;"
        src="https://www.youtube.com/embed/vgrqw2HLFJY?si=YMZ3WdnaVVBPpWdr" 
        title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; 
        encrypted-media; gyroscope; picture-in-picture; web-share" 
        allowfullscreen></iframe>
    </div>
                                                                                
Tutorial de uso da API 
******************************

Ao acessar a `página da API`_, há dois blocos: o *default* e os *schemas*. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/api-publica/pagina-api

Os **schemas** descrevem os objetos e seus detalhes. Por meio deles, pode-se conhecer
os dados que o Querido Diário armazena e seus tipos. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/api-publica/api-gazetteitem

Já em **default**, ficam os *endpoints* que são as vias de acesso aos dados. Para 
experimentar um deles, selecione-o e clique no botão "Try it out", assim os
campos do *endpoint* ficarão disponíveis para interação.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/api-publica/api-tryitout

Para esta experimentação, fazemos uma busca por "orçamento" nos diários oficiais 
do município de Batalha (AL) por meio do *endpoint* ``/gazettes``. Ao clicar para 
executar, um acesso é feito à API Pública e uma *response* é exibida. 

.. important::
    Tomamos um *endpoint* de exemplo, entretanto todos os demais *endpoints* são 
    simuláveis com a mesma mecânica. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/api-publica/retorno-da-api.png

No campo **response body** é possível conferir o resultado da pesquisa: quantos 
arquivos atendem a busca, um pequeno trecho (ou excerto) do texto de cada diário 
oficial onde a palavra-chave se encontra, informações sobre ele, como data, a fonte
do arquivo na nuvem de armazenamento do Querido Diário, o número da edição, etc. 

Porém, essas informações também podem ser obtidas pela :doc:`interface-de-busca` 
com uma navegação muito mais amigável. Aqui, o que é de interesse, de fato, é a URL
gerada no campo **Response URL**. Nossa busca-exemplo gera a URL a seguir. Experimente 
clicar e ver o que ela disponibiliza: 

https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706&querystring=or%C3%A7amento&excerpt_size=500&number_of_excerpts=1&size=10

Esta URL é um *link* permanente para a pesquisa com configurações específicas. Ela 
pode ser adicionada a *scripts*, como códigos de análise de dados, para fazer 
uso automatizado dos dados do Querido Diário. 

Entendendo a Response URL
---------------------------

Navegar pela página da API, experimentando diferentes *endpoints*, configurados 
com diferentes campos é um passo inicial que possibilita maior familiaridade com 
como os dados do Querido Diário são acessados e percebendo os padrões de 
preenchimento. Continuando com o exemplo acima, a URL gerada explicita os campos: 

... a API foi acessada: ``https://queridodiario.ok.org.br/api/``

... no ponto de acesso "gazettes": ``https://queridodiario.ok.org.br/api/gazettes?``

... buscando pelo território de código IBGE "2700706": ``https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706``

... usando a *string* de busca "orçamento": ``https://queridodiario.ok.org.br/api/gazettes?territory_ids=2700706&querystring=or%C3%A7amento``

... e assim por diante.

Ao ir se acostumando com os padrões de construção da URL, essa etapa de usar a página
da API para gerar uma URL fixa, criada para uma busca muito específica, não será 
mais necessária. É possível partir de uma generalização e automatizar o preenchimento
dos campos, criando URLs personalizadas em tempo de execução do *script*. 

Um exemplo, usando o *endpoint* ``gazettes/``, com destaque aos locais onde o 
*script* preencheria os campos: 

``https://queridodiario.ok.org.br/api/gazettes?territory_ids={**CÓDIGO IBGE**}querystring={**PALAVRAS-CHAVE**}&excerpt_size={**VALOR**}&number_of_excerpts={**VALOR**}&size={**VALOR**}``

.. REFERÊNCIAS
.. _página da API: https://queridodiario.ok.org.br/api/docs
.. _Python: https://www.python.org/
.. _FastAPI: https://fastapi.tiangolo.com/
.. _Swagger: https://swagger.io/