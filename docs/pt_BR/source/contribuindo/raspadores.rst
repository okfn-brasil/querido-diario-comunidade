Raspadores
######################

Responsáveis pela etapa de coleta de dados na :doc:`../entendendo/arquitetura`, 
os raspadores são robôs programados para obter os diários oficiais e suas informações
dos *sites* publicadores. 

O principal desafio dessa etapa é o de ter cada vez mais robôs para aumentar a cobertura 
do banco de dados do projeto, buscando alcançar todos os 5570 municípios brasileiros. 

Entendendo o código 
************************

Os raspadores são desenvolvidos em `Python`_ usando o *framework* `Scrapy`_ seguindo 
o paradigma de Orientação a Objetos. Por isso, os principais componentes de código 
são classes, e elas são utilizadas pelo `Scrapy`_ para realizar as coletas.  

O diagrama resume a relação entre as classes raspadoras. Cada uma é aprofundada 
nos tópicos enumerados correspondentes desta seção e, ao fim, é apresentada a ordem 
em que seus componentes são acionados quando um comando de raspagem é engatilhado. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/scrapers-class-hierarchy_ptbr.png
    :alt: [TODO]

1. O framework Scrapy
========================

O *framework* de raspagem utilizado é o `Scrapy`_. Isso quer dizer que o Scrapy já 
oferece alguns recursos genéricos, restando ao Querido Diário adaptá-los para o 
contexto mais específico do projeto. Para o Scrapy, cada *script* de raspagem se 
chama **spider**, título que aparecerá diversas vezes também no Querido Diário.

.. seealso::
  Para conhecer mais, a documentação oficial do `Scrapy`_ tem uma seção de primeiros 
  passos, com conteúdos como `uma primeira olhada no Scrapy (ENG)`_ e `tutorial Scrapy (ENG)`_.

2. A classe Gazette
=====================

Tendo em vista que coletar os arquivos de diários oficiais é o objetivo central, 
a classe `Gazette`_ (diário oficial em inglês) foi criada para vincular cada 
edição às suas informações complementares (metadados). Esta classe é criada a 
partir de `scrapy.Item`_ e é composta por atributos personalizados para o Querido 
Diário. 

.. class:: Gazette (date, edition_number, is_extra_edition, power, file_urls )

   .. attribute:: date
   
      :tipo: `datetime.date()`_
      :definição: Data de publicação informada no *site* publicador de um diário oficial. Em caso de ausência dessa informação, o raspador **não deve ser integrado**.
   
   .. attribute:: edition_number
   
      :tipo: **string**
      :definição: Número informado no *site* publicador de um diário oficial. Em caso de ausência dessa informação, pode ser preenchido com vazio (``""``).
   
   .. attribute:: is_extra_edition
   
      :tipo: **boolean**
      :definição: Informação do *site* publicador se a edição de um diário oficial é extra. Costuma ser identificado pela presença de termos como "Extra", "Extraordinário", "Suplemento". Em caso de ausência dessa informação, deve ser fixado em ``False`` por padrão (*hard coded*). 
   
   .. attribute:: power
   
      :tipo: **string**
      :opções: ``'executive'`` ou ``'executive_legislative'``
      :definição: Informação se o conteúdo do diário oficial é do poder executivo exclusivamente ou se aparecem atos oficiais do poder legislativo também.
   
      Há casos onde é possível ser coletado no site, porém tende a ser um atributo de preenchimento manual pela pessoa desenvolvedora (*hard coded*) a partir da consulta em alguns diários oficias.
       
   .. attribute:: file_urls
   
      :tipo: **list[string]**
      :definição: URL para *download* do arquivo correspondente a edição do diário oficial. Costuma ser apenas uma URL.


3. A classe BaseGazetteSpider
===============================

`BaseGazetteSpider`_ é a classe-mãe para todos os raspadores. Ela é criada a partir 
de `scrapy.Spider`_, em uma relação de herança. Juntas, `scrapy.Spider`_ e *BaseGazetteSpider* 
criam uma lista de elementos obrigatórios para os raspadores :class:`UFMunicipioSpider` 
implementarem. 

4. As classes UFMunicipioSpider
=================================

Para cada município, uma classe :class:`UFMunicipioSpider` correspondente deve existir 
no diretório de `spiders`_. Estruturalmente, a :class:`UFMunicipioSpider` faz uma 
ponte entre as duas classes apresentadas acima: 

a. É criada a partir de **BaseGazetteSpider**, herdando a exigência de implementar 
elementos dela.

b. E termina construindo um objeto :class:`Gazette` para obter o diário oficial e 
seus metadados.

.. class:: UFMunicipioSpider(BaseGazetteSpider)

   .. attribute:: name

      :procedência: `scrapy.Spider.name`_
      :tipo: **string**
      :definição: Nome do raspador no formato ``uf_nome_do_municipio``. É usado no comando de execução da *spider*.
   
   .. attribute:: TERRITORY_ID

      :procedência: `BaseGazetteSpider`_
      :tipo: **string**
      :definição:  Código do IBGE para o município conforme listado no `arquivo de territórios`_.
   
   .. attribute:: allowed_domains
   
      :procedência: `scrapy.Spider.allowed_domains`_
      :tipo: **list[string]**
      :definição:  Domínios em que a *spider* está autorizada a navegar. Evita que a *spider* visite outros *sites* indevidamente.
       
   .. attribute:: start_urls

      :procedência: `scrapy.Spider.start_urls`_
      :tipo: **list[string]**
      :definição: URL da página onde ficam os diários oficiais no *site* publicador. É apenas uma URL e não costuma ser a *homepage* do *site*.
      :exigência: Opcional. Saiba mais em :ref:`start-urls-ou-start-requests`
   
   .. attribute:: start_date

      :procedência: `BaseGazetteSpider`_
      :tipo: `datetime.date()`_      
      :definição: Data da primeira edição de diário oficial disponibilizado no *site* publicador.
   
   .. attribute:: end_date

      :procedência: `BaseGazetteSpider`_
      :tipo: `datetime.date()`_      
      :definição: Data da última edição de diário oficial disponibilizado no *site* publicador. 
      :exigência: Implícito. Por padrão, é preenchido automaticamente com a data da execução do raspador (``datetime.today().date()``). Só é explicitado em raspadores que coletam :ref:`sites descontinuados<sites-descontinuados>`.
   
   .. method:: start_requests()
   
      :procedência: `scrapy.Spider.start_requests()`_
      :returns: `scrapy.Request()`_
      :definição: Método para criar URLs para as páginas de diários oficiais do *site* publicador
      :exigência: Opcional. Saiba mais em :ref:`start-urls-ou-start-requests`
   
   .. method:: parse(response)
   
      :procedência: `scrapy.Spider.parse()`_
      :returns: :class:`Gazette`
      :definição: Método que implementa a lógica de extração de metadados a partir do texto da `Response`_ obtida do *site* publicador. É o *callback* padrão.
      :exigência: Obrigatório. Porém, é possível que tenha :ref:`outro nome<parse-alternativo>` 


Esqueleto de uma UFMunicipioSpider
-------------------------------------

Com isso, já é possível visualizar um esqueleto de todos os *scripts* de raspadores 
e saber suas partes básicas. Note como todos os componentes de :class:`UFMunicipioSpider` 
e :class:`Gazette` aparecem a seguir.

.. code-block:: python

    from datetime import date
    
    import scrapy import Request

    from gazette.items import Gazette
    from gazette.spiders.base import BaseGazetteSpider

    class UFMunicipioSpider(BaseGazetteSpider):  
        name = "uf_nome_do_municipio"
        TERRITORY_ID = ""                                                      
        allowed_domains = [""]
        start_urls = [""]
        start_date = date()                   

        def start_requests(): # (caso necessário)
            # Lógica de geração de URLs 
            # ...

            yield Request()

        # ... métodos auxiliares opcionais ...

        def parse(self, response):
            # Lógica de extração de metadados
            
            # partindo de response ...
            # 
            # ... o que deve ser feito para coletar DATA DO DIÁRIO?
            # ... o que deve ser feito para coletar NÚMERO DA EDIÇÃO?
            # ... o que deve ser feito para coletar se a EDIÇÃO É EXTRA? 
            # ... o que deve ser feito para coletar a URL DE DOWNLOAD do arquivo?
            # ... o que deve ser feito para coletar PODER?

            yield Gazette(
                date = date(),  
                edition_number = "",       
                is_extra_edition = False,
                file_urls = [""],    
                power = "",
            )

.. _classe-sistema:

5. As classes BaseSistemaSpider
====================================

Em alguns casos, fica visualmente perceptível que o *site* de diferentes cidades 
tem o mesmo *layout*. `Acajutiba (BA)`_, `Cícero Dantas (BA)`_ e `Monte Santo (BA)`_
são exemplos disso. Essa situação acontece quando municípios adotam a mesma solução 
publicadora de diários ou de desenvolvimento de *sites*.

A implicação para o repositório é que o código das classes *BaAcajutibaSpider*,
*BaCiceroDantasSpider* e *BaMonteSantoSpider* seriam enormemente parecido: seus 
raspadores "navegariam" nos *sites* da mesma maneira para obter metadados que ficam 
na mesma posição. 

Para simplificar a situação, evitando repetição de código e facilitando a adição 
de novos raspadores a partir do padrão conhecido - *até porque estes são 3 casos,
quantos outros existem?* -, temos as classes **BaseSistemaSpider**. 

.. note::
    Adotamos a terminologia **sistema replicável** para nos referir ao padrão e 
    **município replicado** os municípios que o utilizam.

.. important::
    Vários padrões são conhecidos. Seus códigos estão no `diretório de bases`_ e 
    seus *layouts* na :doc:`Lista de Sistemas Replicáveis<lista-sistemas-replicaveis>`

Esqueleto de uma BaseSistemaSpider
--------------------------------------

Como parte da família de *spiders* do Querido Diário, a **BaseSistemaSpider** segue
a mesma estrutura: é criada a partir de **BaseGazetteSpider** e finaliza construindo 
objeto **Gazette**. 

Porém, a classe que seria um raspador para um município individual é dividida em 
duas: **BaseSistemaSpider**, onde ficam os recursos comuns para vários casos, e 
:class:`UFMunicipioSpider`, onde ficam as partes específicas para cada caso. 

De modo geral, os métodos :meth:`~UFMunicipioSpider.start_requests`, :meth:`~UFMunicipioSpider.parse` 
e a criação de objetos :class:`Gazette` ficam em **BaseSistemaSpider** e os atributos 
:attr:`~UFMunicipioSpider.TERRITORY_ID`, :attr:`~UFMunicipioSpider.name`, 
:attr:`~UFMunicipioSpider.start_date` e :attr:`~Gazette.power`, por serem dados 
particulares de cada município, ficam em :class:`UFMunicipioSpider`.  

BaseSistemaSpider
^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from datetime import date
    
    import scrapy import Request

    from gazette.items import Gazette
    from gazette.spiders.base import BaseGazetteSpider

    class BaseSistemaSpider(BaseGazetteSpider):

        def start_requests(self): 
            # Lógica de geração de URLs 
            # ...

            yield Request()

        # ... métodos auxiliares opcionais ...

        def parse(self, response):
            # Lógica de extração de metadados
            
            # partindo de response ...
            # 
            # ... o que deve ser feito para coletar DATA DO DIÁRIO?
            # ... o que deve ser feito para coletar NÚMERO DA EDIÇÃO?
            # ... o que deve ser feito para coletar se a EDIÇÃO É EXTRA? 
            # ... o que deve ser feito para coletar a URL DE DOWNLOAD do arquivo?
            # ... o que deve ser feito para coletar PODER?

            yield Gazette(
                date = date(),  
                edition_number = "",       
                is_extra_edition = False,
                file_urls = [""],    
                power = self.power,
            )

.. _exemplo-municipio-replicado:

.. attention::
    Tenha em mente que esses esqueletos são em função de onde cada componente *tende* 
    a aparecer, porém não são soluções fixas. Alguns elementos mudam a depender 
    da situação e das escolhas de desenvolvimento. 

UFMunicipioSpider para uma BaseSistemaSpider genérica
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Neste caso, :class:`UFMunicipioSpider` fica bastante simplificada uma vez que herdará 
os métodos implementados em **BaseSistemaSpider**, restando apenas implementar os 
demais atributos.

.. code-block:: python
  :emphasize-lines: 3,5

    from datetime import date

    from gazette.spiders.base.sistema import BaseSistemaSpider

    class UFMunicipioSpider(BaseSistemaSpider):
        name = "uf_nome_do_municipio"
        TERRITORY_ID = ""    
        allowed_domains = [""]
        power = ""
        start_date = date()                                                                          

6. Fluxo de execução 
======================

Idealmente, ao executar :ref:`o comando de raspagem<executando>` para 
um raspador qualquer, o Scrapy aciona seu método :meth:`~UFMunicipioSpider.start_requests()` 
fazendo uma requisição inicial para a URL definida no atributo :attr:`~UFMunicipioSpider.start_urls`.
A `Response`_ recebida é entregue ao método de *callback* :meth:`~UFMunicipioSpider.parse()`,
onde metadados são coletados, um objeto :class:`Gazette` é construído e é enviado 
ao `motor do Scrapy`_ para executar, de fato, a ação de baixar o arquivo do diário 
oficial.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/scrapers-default-flow_ptbr.png
    :alt: [TODO]

Há dois contextos em que, por exigência da situação, esse fluxo pode não acontecer 
do jeito ilustrado:

1. quando :class:`UFMunicipioSpider` tem seu próprio ``start_requests()``, não sendo usado o que existe em `scrapy.Spider`_.
2. quando o :meth:`~UFMunicipioSpider.parse()` não é usado como *callback* padrão.


.. _start-urls-ou-start-requests:

Quando usar :attr:`~UFMunicipioSpider.start_urls` ou :meth:`~UFMunicipioSpider.start_requests()`
--------------------------------------------------------------------------------------------------

Primeiro, é importante destacar que um método ``start_requests()`` sempre existe, 
pois já é implementado pelo Scrapy. A questão aqui é quando a implementação padrão, 
ilustrada acima, não é suficiente. 

Por exemplo, um *site* que organiza diários por data ou intervalo, a URL da requisição
inicial pode precisar preencher campos de data. Ou um que atenda vários municípios
ou poderes, pode ser necessário código identificador. Ainda, se uma API for encontrada, 
a URL muda a depender dos *endpoints* e seus campos. Outros diversos casos acontecem 
e nessas situações, a opção nativa não serve por ser muito restrita, uma vez que o 
quê ela espera receber é uma URL já fixada. 

Quando a situação demanda criação de várias URLs iniciais e/ou parâmetros de contexto, 
a operação padrão do método ``start_requests()`` deve ser sobreescrita com um 
:meth:`~UFMunicipioSpider.start_requests()` novo dentro do raspador que implemente 
a lógica particular de construção dinâmica de URLs. Com :meth:`~UFMunicipioSpider.start_requests()` 
gerando URLs, o atributo :attr:`~UFMunicipioSpider.start_urls` não tem porque existir. 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Exemplo com :attr:`~UFMunicipioSpider.start_urls`
     - Exemplo com :meth:`~UFMunicipioSpider.start_requests()`
   * - `raspador para Paulínia-SP`_
     - `raspador para Barreiras-BA`_


O método :meth:`~UFMunicipioSpider.parse()`
---------------------------------------------

Por padrão, o fluxo de execução desemboca em :meth:`~UFMunicipioSpider.parse()`. 
Para implementar um ``parse()`` que cumpra bem seu papel de obtenção de metadados 
a partir do conteúdo textual da `Response`_, é importante que a pessoa desenvolvedora 
saiba inspecionar uma página *web* a fim de identificar seletores e construir 
`expressões regulares (RegEx)`_ convenientes para serem usadas no método. 

O Scrapy conta com `seletores (ENG)`_ nativos que podem ser testados usando o 
`Scrapy shell`_. Já para expressões regulares, é comum o uso da biblioteca `re`_ 
de Python e, como sugestão, *sites* que testam *strings regex*, como `RegExr`_, podem
ajudar.

Menos comuns, mas por vezes necessárias, outras bibliotecas já estão entre as dependências 
do repositório e podem ser úteis: 

- `dateparser`_ para tratar datas em diferentes formatos;
- `dateutil`_ para, principalmente, criar datas recorrentes;
- `urllib.parse`_ para tratar, desmontando e remontando, URLs de forma padronizada;
- `chompJS`_ para transformar objetos JavaScript em estruturas Python.

.. seealso::
    Materiais complementares são indicados na seção :ref:`Aprenda mais sobre os raspadores<aprenda>` 

.. _parse-alternativo:

Quando o :meth:`~UFMunicipioSpider.parse()` não é o método de *callback* 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

O :meth:`~UFMunicipioSpider.parse()` só é o método padrão para o qual a `Response`_ é
enviada por ser assim que o método ``start_requests()`` nativo do Scrapy define. 
Porém, quando for o caso do :ref:`raspador implementar um start_requests() próprio<start-urls-ou-start-requests>`,
pode ser opção da pessoa desenvolvedora indicar outro método como *callback*. O 
`raspador para Macapá-AP`_ é um exemplo disso.

.. caution::
  Um método nomeado como :meth:`~UFMunicipioSpider.parse()` pode não existir, mas 
  o papel que espera-se que ele cumpra segue necessário e deve ser realizado pelo 
  novo *callback* ou outros métodos auxiliares adicionados ao raspador. 


Contribuindo com raspadores
*******************************

Passos iniciais
=================

Antes de colocar a mão na massa, é necessário configurar o ambiente de desenvolvimento
e escolher um município para o qual contribuir. Lembre-se de seguir o :doc:`guia-de-contribuicao` 
durante as interações no repositório. 

- **Repositório**: https://github.com/okfn-brasil/querido-diario
- Passo 1: `Como configurar o Ambiente de Desenvolvimento`_
- Passo 2: `Como escolher uma tarefa para fazer`_

.. danger::
  Apenas casos de :ref:`diários individuais<tipo-diarios>` estão sendo integrados.

Desenvolvendo raspadores
==============================

O desenvolvimento de raspadores é norteado pelo interesse de coletar **todos**
os diários oficiais fazendo o **menor número de requisições possível** ao *site* 
publicador, evitando correr o risco de sobrecarregá-lo. Queremos, também, 
que o raspador **controle o período da coleta** dentro da série histórica completa 
de disponibilização. 

Por isso, :class:`UFMunicipioSpider` tem os atributos :attr:`~UFMunicipioSpider.start_date` e 
:attr:`~UFMunicipioSpider.end_date` e seus métodos :meth:`~UFMunicipioSpider.parse()`
e/ou :meth:`~UFMunicipioSpider.start_requests()` devem ter, onde e como for conveniente,
condicionais para a data sendo raspada, mantendo-a dentro do intervalo de interesse 
ao seguir executando. 

.. tip::
    Durante o desenvolvimento, para evitar fazer requisições repetidas nos *sites* 
    é possível utilizar a configuração `HTTPCACHE_ENABLED`_ do Scrapy. Isso também 
    faz com que as execuções sejam mais rápidas, já que todos os dados ficam armazenados 
    localmente.

Estratégias comuns
----------------------

Navegar pelo diretório de `spiders`_ lendo o código de raspadores existentes
ajudará o desenvolvimento de novos raspadores, principalmente com soluções para 
situações frequentes. Conheça alguns casos:

Paginação
^^^^^^^^^^^^^^^^^^^^

Quando as publicações de diários oficiais estão separadas em várias páginas referenciadas 
por botões como "página 1", "página 2", "próxima página". 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Caso com Paginação
     - Raspador
   * - `Site de Manaus-AM`_
     - `am_manaus.py`_
   * - `Site de Sobral-CE`_
     - `ce_sobral.py`_
   * - `Site de João Pessoa-PB`_
     - `pb_joao_pessoa.py`_


Filtro por data
^^^^^^^^^^^^^^^^^^^^

Quando o *site* publicador oferece um formulário com campos de data para filtrar 
as publicações. 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Caso com Filtro
     - Raspador
   * - `Site de Sobral-CE`_
     - `ce_sobral.py`_
   * - `Site de Salvador-BA`_
     - `ba_salvador.py`_

Presença de APIs
^^^^^^^^^^^^^^^^^^^^

Quando é percebido que as requisições do *site* se dão por meio de APIs Públicas 
devolvendo um formato JSON. 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Caso com API
     - Raspador
   * - `Exemplo de acesso à API em Natal-RN`_
     - `rn_natal.py`_


Um município, diários oficiais em diferentes lugares 
=======================================================

É comum existir mais de um lugar por onde o mesmo município disponibiliza diários 
oficiais, fazendo ser de interesse cobrir a raspagem de todos eles para o Querido
Diário ter o histórico completo. Entretanto, algumas coisas devem ser levadas em 
consideração: 

- Essa situação parece acontecer pelo município ter um **site vigente**, em uso atualmente onde publica edições recentes, e um ou mais **sites descontinuados**, onde ficam edições antigas publicadas no passado.

  - Menos comum, mas acontece também de dois *sites* serem vigentes. É considerado apenas o que tiver a maior série histórica.
  - Para *sites* descontinuados, o código só será mantido no repositório enquanto o *site* estiver ativo. No caso de um *site* ser abandonado pelo município, o código pode ser excluído ou sobreescrito com código novo.

- A integração dos raspadores se dará, sempre, em prol de **garantir que a cobertura fique consistente**. 

  - Ou seja, a partir do site vigente rumo aos sites descontinuados, de forma a não esburacar a sequência histórica armazenada no banco de dados e, consequentemente, oferecida aos usuários para busca. 

.. important::
  Isso não significa que, para uma contribuição ser realizada, é necessário encontrar todos os *sites* que um município já usou. Mas significa que contribuições que não seguem essa ordem ficarão em espera.
  
- A organização do repositório segue a diretriz **um raspador para um layout**.

  - A decisão visa manter a spider atomizada, sem lógicas diferentes misturadas em um mesmo raspador, tornando o código mais fácel de manter.
  - Então, na situação de um município usar mais *sites* ou seções/páginas completamente diferentes entre si ainda que em um mesmo *site*, arquivos `.py` separados devem ser criados para coletar cada um.

.. _sites-descontinuados:

Modificações
----------------

A primeira integração - do raspador que coleta o *site* vigente - segue o fluxo normal 
de contribuição: demanda a implementação de uma :class:`UFMunicipioSpider` padrão. Ao 
serem encontrados *sites* descontinuados, algumas adaptações devem ser feitas: 

- As nomenclaturas para o arquivo, :class:`UFMunicipioSpider` e :attr:`~UFMunicipioSpider.name` recebem ``_<ano-inicial>`` ao fim; ficando, por exemplo: `pe_recife_2022`
- O atributo :attr:`~UFMunicipioSpider.end_date` deve ser explicitado, usando a data da última edição disponível no *site* descontinuado
- Alterações no nome do arquivo devem ser feitas de forma a preservar a conexão com o histórico do versionamento. 

.. code-block:: sh

  git mv <nome-atual-do-arquivo> <novo-nome-do-arquivo>


.. _executando:

Executando raspadores
==========================

Para executar um raspador, utiliza-se o comando `crawl`_. Este é um dos 
`comandos padrão (ENG)`_ oferecidos pelo *framework* Scrapy e conhecer os demais 
também pode ajudar a desenvolver raspadores com mais facilidade.

O comando de raspagem deve ser executado no diretório ``data_collection/`` com o 
ambiente de desenvolvimento ativo. Nele, surgirá um arquivo SQLite ``queridodiario.db``
e um diretório ``data/``, organizado por :attr:`~UFMunicipioSpider.TERRITORY_ID` 
e :attr:`~Gazette.date`, onde ficam os arquivos de diários oficiais baixados.

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio [+ opções]

- ``-a start_date=AAAA-MM-DD`` define novo valor para :attr:`~UFMunicipioSpider.start_date`
- ``-a end_date=AAAA-MM-DD`` define novo valor para :attr:`~UFMunicipioSpider.end_date`
- ``-s LOG_FILE=nome_arquivo.log`` salva as mensagens de *log* em arquivo de texto
- ``-o nome_arquivo.csv`` salva a lista de diários oficiais e metadados coletados em arquivo tabular

Testes exigidos
------------------------

Uma pessoa contribuidora deve testar seu código para os 3 casos abaixo e anexar os 
:ref:`arquivos auxiliares<arquivos-auxiliares>` aos comentários da *pull request*, 
mostrando que seu raspador funciona conforme esperado e agilizando o processo de revisão. 

Coleta da última edição
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Veja a data da edição mais recente no *site* publicador e execute a raspagem a partir dela. 

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio -a start_date=AAAA-MM-DD
  
Coleta de intervalo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Escolha um intervalo (uma semana, algumas semanas, um mês, alguns meses, etc) e 
execute a coleta desse intervalo arbitrário. 

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio -a start_date=AAAA-MM-DD -a end_date=AAAA-MM-DD

Coleta completa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Execute a coleta sem filtro por datas para obter toda a série histórica de edições no *site* publicador.  

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio

.. admonition:: Conheça as rotinas de raspagem

  **Coleta diária**: Em produção, a coleta da última edição é feita para todos os 
  raspadores diariamente a partir das 18h. 

  **Coleta mensal**: Em produção, a coleta de intervalo é engatilhada, todo dia 1º,
  como uma rotina de redundância por precaução, evitando a perda de edições retardatárias. 
  Também é usada em momentos de manutenção de raspadores para obtenção de intervalos 
  faltantes. 

  **Coleta completa**: Em produção, a coleta completa é executada sempre que um 
  novo raspador é integrado ao projeto.

.. _verificando:

Validando raspagens
==========================

Verificar uma coleta quer dizer vasculhar os arquivos que uma raspagem produz - 
diários oficiais, tabela da coleta e *log* - conferindo se foi coletado tudo disponível 
e da forma como se espera.

.. _entendendo-a-situacao:

Entendendo a situação
--------------------------

Ao encontrar uma situação que parece indicar que o raspador está perdendo dados 
na coleta é importante buscar saber se existe uma **solução técnica** (= há algo 
a ser feito) ou se é **fora do controle do Querido Diário** (= não há algo a ser 
feito). 

Exemplos são: se as edições número 5 e 7 foram obtidas, mas a 6 não; ou o *log* indica 
erro de acesso em uma *URL*. Se, ao navegar manualmente o site, a edição 6 é encontrada 
ou a *URL* abre quando copiada e colada no navegador, é confirmação de que 
o problema é no raspador, que deve ser corrigido. Se não deu certo, está confirmado 
que o problema não é do raspador - é do site ou da prefeitura. 

Outros contextos podem existir e, sempre, a abordagem será a de **comparar com 
a fonte** (o site publicador). O código não estará impedido de ser integrado quando 
a responsabilidade pelo problema for o site ou a prefeitura. 


Conferindo arquivos
-------------------------------------

Algumas dicas para verificações nos arquivos de saída das raspagens. 

Diários Oficiais (``data_collection/data/``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Extensão dos arquivos, que devem ser ``.pdf``, ``.doc`` ou ``.docx``;
- Conteúdo é o de um diário oficial realmente, não outro tipo de documento;
- Se a data dentro do arquivo corresponde à data do diretório em que está;
- Se o documento corresponde exclusivamente ao município.

.. attention::
  É comum que municípios reproduzam o diário da associação municipal ao qual 
  pertencem ao invés de um documento individual. As edições mais antigas do 
  `diário oficial de Milagres do Maranhão-MA`_ é um exemplo. Estes casos serão não 
  devem ser integrados.

.. _arquivos-auxiliares:

Tabela da coleta (arquivo ``.csv``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Coerência do metadados: :attr:`~Gazette.date`, :attr:`~Gazette.edition_number`, :attr:`~Gazette.is_extra_edition`, :attr:`~Gazette.power` e :attr:`~Gazette.file_urls`. Como o arquivo é uma tabela, é útil ordenar as colunas para fazer a conferência;
- Se a primeira e a última edição (:attr:`~UFMunicipioSpider.start_date` e :attr:`~UFMunicipioSpider.end_date`) estão dentro do intervalo de coleta definido, ou seja, se o filtro por data implementado funciona;
- Se faltam certas datas ou números de edição que possam indicar que arquivos não foram coletados.

.. attention::
  É importante ter atenção na ordenação das datas para verificar a ausência de documentos
  por longos períodos (semanas ou meses). Uma situação dessa sendo encontrada, deve 
  ser avisada na *PR* e discutida. A depender do caso, pode impossibilitar que o 
  raspador seja integrado.

Log (arquivo ``.log``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Configurações: parte inicial, até** ``INFO: Spider opened``

  - Lista as configurações internas sendo usadas, como as versões das bibliotecas, módulos, extensões, nome e intervalo do raspador, etc.
  - Acaba sendo uma seção mais "administrativa" e, normalmente, não tem o quê ser verificado nela. 

- **Requisições: parte do meio, de** ``INFO: Spider opened`` **a** ``INFO: Closing spider``

  - Lista as requisições de acesso ao site (``DEBUG: Crawled ...``) e as requisições de download de arquivos (``DEBUG: File (downloaded)...``) feitas pelo raspador.
  - Visto que registra *todas* as requisições feitas, esta seção normalmente é usada para se investigar detalhes de alguma situação mais particular.    

- **Estatísticas: parte final, de** ``INFO: Closing spider`` **a** ``INFO: Spider closed (finished)``

  - Lista dados gerais da coleta e traz uma análise sobre o comportamento do raspador.
  - Sendo um relatório resumido, é por onde uma validação/revisão normalmente começa. Olha-se o resumo em busca de algum ponto de atenção e, caso haja, parte-se para entender o que houve.

Explorando o log
=============================

Na seção de estatísticas do *log*, algumas naturezas de informação são exibidas 
juntas, com informações gerais seguidas das específicas. Nos referimos a isso 
como "bloco temático" nesta seção, trazendo alguns aspectos do que se analisar no 
*log*.

.. note::
  Tenha em mente que essas serão apenas dicas para começar, visto que o *log* do 
  Scrapy tem uma lógica própria. Não há intenção de se esgotar todas as possibilidades,
  mas de permitir uma familiarização inicial e apontar os indícios mais frequentes. 

.. important::
  Lembre-se que, conforme a seção :ref:`Entendendo a situação<entendendo-a-situacao>` orienta,
  nem sempre a ocorrência de certas mensagens significam problemas, de fato. Situações 
  específicas são conversadas e, nem sempre, impossibilita a integração do raspador. 
  O importante é evidenciar os casos para discussão. `Este é um exemplo`_.

Bloco de informações gerais
--------------------------------

O bloco de contabilizações gerais é o primeiro passo para se averiguar problemas.
Nele, o contador de ``ERROR`` ou ``WARNING`` existindo (se não existir, é por que 
é igual à 0) indicam pontos de atenção, permitindo fazer buscas globais no
arquivo (*ctrl + f*) por esses termos.

.. code-block:: sh

  log_count/DEBUG: n
  log_count/ERROR: n
  log_count/INFO: n
  log_count/WARNING: n

Bloco de status
----------------------------

O bloco de *status* mostra a quantidade de *responses* obtidas e a situação delas. 
É desejado que todas sejam 200 (no exemplo, n = x), código que indica sucesso. 

.. code-block:: sh

  # como aparece na seção de estatísticas... 

  downloader/response_count: n                      <total>
  downloader/response_status_count/'CODIGO': z      <parcela de total>
  downloader/response_status_count/200: x           <parcela de total>
  downloader/response_status_count/302: y           <parcela de total>

Caso outros ``CODIGO`` forem contabilizados, a dica é fazer uma busca global no 
arquivo, usando o código entre parênteses, para encontrar a situação específica. 

.. code-block:: sh

  # ... e como reflete na seção de requisições 

  Crawled (200) <GET https://...> 
  Redirecting (302) to <GET https://...> from <GET http://...>

Códigos específicos devem ser consultados na `codificação oficial de respostas HTTP`_. 
Eles seguem o seguinte agrupamento: 

- Respostas Informativas (100 à 199)
- Respostas bem-sucedidas (200 à 299)
- Mensagens de redirecionamento (300 à 399)
- Respostas de erro do cliente (400 à 499)
- Respostas de erro do servidor (500 à 599)

.. note::
  - códigos de redirecionamento costumam aparecer sem que isso indique um problema; entretanto, muitas ocorrências pode indicar que alguma escolha de desenvolvimento precisa ser revista;
  - códigos de erro do servidor costumam indicar que o raspador está exigindo demais do servidor. Será necessário adicionar `configurações específicas`_ para diminuir o ritmo de raspagem. Este é um exemplo de `raspador com custom_settings`_. 

Bloco de tentativas repetidas
-----------------------------------

O Scrapy está configurado para tentar até 3 vezes uma requisição que não obteve 
sucesso. O bloco de tentativas repetidas contabiliza esses casos, indicando também 
os ``MOTIVO``.

.. code-block:: sh

  # como aparece na seção de estatísticas... 

  retry/count: n,                                     <total>
  retry/reason_count/'MOTIVO': y,                     <parcela de total>
  retry/reason_count/500 Internal Server Error: x,    <parcela de total>

Tentativas repetidas são sinalizadas com ``WARNING`` na primeira e segunda vezes, 
se tornando ``ERROR`` apenas na terceira. Uma *URL* não acessada significa, em 
último caso, arquivos não coletados. 

.. code-block:: sh

  # ... e como reflete na seção de requisições

  Retrying <GET https://...> (failed 'N' times)

Bloco de validações
-------------------------------------

No bloco de validações, pode aparecer indícios de que itens foram abandonados (*dropped*)
e o ``MOTIVO`` do abandono.  

.. code-block:: sh

  # como aparece na seção de estatísticas... 

  spidermon/validation/items: n                     <total>
  spidermon/validation/items/dropped: x             <parcela de total>
  ...
  spidermon/validation/fields: n                    <total>
  spidermon/validation/fields/errors/'MOTIVO': y    <parcela de total>

Na seção de requisições do *log*, aparecerá informações específicas daquele arquivo 
abandonado no padrão abaixo. Usar *dropped* ou *failed* na busca global ajuda a 
encontrar os casos.

.. code-block:: sh

  # ... e como reflete na seção de requisições

  WARNING: Dropped: Validation failed!
  {_validation: < mensagem que tem muitas coisas, entre elas 'MOTIVO' >,
  date: ...,
  edition_number: ...,
  file_urls: [...],
  files: ,
  is_extra_edition: ...,
  power: ...,
  scraped_at: ,
  territory_id: ...}

.. attention::
  Tenha em mente que abandono de arquivo não é o problema em si, mas a consequência 
  de um problema. 

  Por exemplo, foi tentado 3 vezes baixar um arquivo sem sucesso, isso cria um rastro 
  no bloco de tentativas repetidas (erro) e no bloco de validações. Nesse caso, 
  é possível que todos os campos estejam preenchidos, mas *files* e *scraped_at* 
  não, visto que o arquivo não foi baixado. A ausência de dados esperados gerou 
  o alerta de validação

Erro de Integridade
-------------------------------------

Ocorre quando tenta-se adicionar ao banco de dados ``querido-diario.db`` uma edição 
que já existe armazenada nele. 

.. code-block:: sh

  Something wrong has happened when adding the gazette in the database. 
  Date: 'AAAA-MM-DD'. File Checksum: 'HASH'. 
  Details: (sqlite3.IntegrityError) UNIQUE constraint failed ...

A restrição de exclusividade (*unique constraint*) serve para evitar o registro de 
diários oficiais repetidos, provavelmente por um descuido por parte do município.  

.. note::
  É comum esse erro aparecer ao se executar um raspador mais vezes, especialmente 
  durante testes, sem lembrar de excluir o banco de dados da coleta anterior.


.. _aprenda:

Aprenda mais sobre os raspadores
*******************************************************

Aulas
==============

  `Módulo 3 do Curso Python para Inovação Cívica`_ da `Escola de Dados`_:
    - Aula 1: `Apresentando o Querido Diário`_
    - Aula 2: `Por dentro do Querido Diário`_
    - Aula 3: `Introdução a Orientação a Objeto`_
    - Aula 4: `Por dentro do raspador do Querido Diário`_
    - Aula 5: `Analisando páginas web - Inspecionando elementos`_
    - Aula 6: `Analisando páginas web - Inspecionando a rede`_
    - Aula 7: `Selecionando elementos com XPath`_
    - Aula 8: `Expressões Regulares`_
    - Aula 9: `Traduzindo a análise para um raspador`_
    - Aula 10: `Indo além`_

Sessões gravadas
==============================

  Desenvolvimento
    - Ana Paula Gomes: `Querido Diário, hoje eu tornei um diário oficial acessível`_ 
    - Giulio Carvalho: `Raspador para Petrópolis-RJ do início ao fim`_ (coding dojo)
  Revisão 
    - Renne Rocha

      - `Aldeias Altas-MA`_
      - `Feira de Santana-BA`_
      - `Maceió-AL`_
      - `Boa Vista-RR`_
      - `Teresina-PI`_
      - `João Pessoa-PB`_
      - `Niterói-RJ`_
      - `Macapá-AP`_
      - `Aracaju-SE`_, continuação `Aracaju-SE + Maceió-AL`_
      - `Itaúna-MG`_
      - `Piracicaba-SP`_

Textos
=======================

  - Giulio Carvalho, `Entenda como analisar sites de diários oficiais para raspagem de dados`_
  - Juliana Trevine, `Conheça os desafios de raspagem do Querido Diário`_
  - Ana Paula Gomes, `Quero tornar Diários Oficiais acessíveis. Como começar?`_
  - Lucas Villela, `Monitorando o governo de Araraquara/SP`_
  - José Vanz, `Como funciona o robozinho do Serenata que baixa os diários oficiais?`_

.. Referências

.. para a documentação do Scrapy 
.. _Scrapy: https://scrapy.org/
.. _uma primeira olhada no Scrapy (ENG): https://docs.scrapy.org/en/latest/intro/overview.html
.. _tutorial Scrapy (ENG): https://docs.scrapy.org/en/latest/intro/tutorial.html
.. _scrapy.Spider: https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy-spider
.. _BaseGazetteSpider: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/base/__init__.py
.. _scrapy.Spider.name: https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider.name
.. _scrapy.Spider.allowed_domains: https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider.allowed_domains
.. _scrapy.Spider.start_urls: https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider.start_urls
.. _scrapy.Spider.start_requests(): https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider.start_requests
.. _scrapy.Spider.parse(): https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider.parse
.. _scrapy.Item: https://docs.scrapy.org/en/latest/topics/items.html
.. _scrapy.Request(): https://docs.scrapy.org/en/latest/topics/request-response.html#scrapy.http.Request
.. _HTTPCACHE_ENABLED: https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-enabled
.. _motor do Scrapy: https://docs.scrapy.org/en/latest/topics/architecture.html
.. _comandos padrão (ENG): https://docs.scrapy.org/en/latest/topics/commands.html#available-tool-commands
.. _crawl: https://docs.scrapy.org/en/latest/topics/commands.html#crawl
.. _Response: https://docs.scrapy.org/en/latest/topics/request-response.html#response-objects
.. _Scrapy shell: https://docs.scrapy.org/en/latest/topics/shell.html
.. _configurações específicas: https://docs.scrapy.org/en/latest/topics/settings.html#built-in-settings-reference

.. para o repositório de raspadores
.. _Gazette: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/items.py
.. _spiders: https://github.com/okfn-brasil/querido-diario/tree/main/data_collection/gazette/spiders
.. _sistema replicável DOEM: https://github.com/okfn-brasil/querido-diario/edit/main/data_collection/gazette/spiders/base/doem.py
.. _diretório de bases: https://github.com/okfn-brasil/querido-diario/tree/main/data_collection/gazette/spiders/base
.. _Como escolher uma tarefa para fazer: https://github.com/okfn-brasil/querido-diario/blob/main/docs/CONTRIBUTING.md#como-escolher-uma-issue-para-fazer
.. _Como configurar o Ambiente de Desenvolvimento: https://github.com/okfn-brasil/querido-diario/blob/main/docs/CONTRIBUTING.md#como-configurar-o-ambiente-de-desenvolvimento
.. _arquivo de territórios: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/resources/territories.csv
.. _Quadro de Expansão de Cidades: https://github.com/orgs/okfn-brasil/projects/12
.. _Este é um exemplo: https://github.com/okfn-brasil/querido-diario/pull/1185

.. para exemplos de raspadores ou sites de prefeituras 
.. _Acajutiba (BA): https://doem.org.br/ba/acajutiba/diarios
.. _Cícero Dantas (BA): https://doem.org.br/ba/cicerodantas/diarios
.. _Monte Santo (BA): https://doem.org.br/ba/montesanto/diarios
.. _raspador para Paulínia-SP: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/sp/sp_paulinia.py
.. _raspador para Barreiras-BA: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ba/ba_barreiras.py
.. _raspador para Macapá-AP: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ap/ap_macapa.py
.. _Site de Manaus-AM: http://dom.manaus.am.gov.br/diario-oficial-de-manaus
.. _am_manaus.py: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/am/am_manaus.py
.. _Site de Sobral-CE: https://www.sobral.ce.gov.br/diario/pesquisa/index
.. _ce_sobral.py: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ce/ce_sobral.py
.. _Site de João Pessoa-PB: https://www.joaopessoa.pb.gov.br/doe-jp/
.. _pb_joao_pessoa.py: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/pb/pb_joao_pessoa.py
.. _Site de Salvador-BA: http://www.dom.salvador.ba.gov.br/#
.. _ba_salvador.py: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ba/ba_salvador.py
.. _Exemplo de acesso à API em Natal-RN: https://www.natal.rn.gov.br/api/dom/data/10/2023
.. _rn_natal.py: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/rn/rn_natal.py
.. _diário oficial de Milagres do Maranhão-MA: https://www.milagresdomaranhao.ma.gov.br/diario/diario
.. _raspador com custom_settings: https://github.com/okfn-brasil/querido-diario/blob/d495f90193ae4699a9669cf735abc64f3c128eb6/data_collection/gazette/spiders/base/modernizacao.py#L15

.. links externos
.. _Python: https://www.python.org/
.. _datetime.date(): https://docs.python.org/3/library/datetime.html#datetime.date
.. _expressões regulares (RegEx): https://pt.wikipedia.org/wiki/Express%C3%A3o_regular
.. _seletores (ENG): https://docs.scrapy.org/en/latest/topics/selectors.html
.. _RegExr: https://regexr.com/
.. _re: https://docs.python.org/3/library/re.html
.. _chompJS: https://nykakin.github.io/chompjs/
.. _dateparser: https://dateparser.readthedocs.io/en/latest/
.. _dateutil: https://dateutil.readthedocs.io/en/stable/index.html
.. _urllib.parse: https://docs.python.org/3/library/urllib.parse.html#module-urllib.parse
.. _codificação oficial de respostas HTTP: https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status

.. links da seção aprenda mais
.. _Escola de Dados: https://escoladedados.org/courses/
.. _Módulo 3 do Curso Python para Inovação Cívica: https://www.youtube.com/playlist?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Apresentando o Querido Diário: https://youtu.be/3SCQl4cYB5I?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Por dentro do Querido Diário: https://youtu.be/plvSFl0IcVM?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Introdução a Orientação a Objeto: https://youtu.be/LdHRa3r1VoE?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Por dentro do raspador do Querido Diário: https://youtu.be/FWIezX7xlIY?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Analisando páginas web - Inspecionando elementos: https://youtu.be/8I00gSavbxk?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Analisando páginas web - Inspecionando a rede: https://youtu.be/RAWhGIEnxxw?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Selecionando elementos com XPath: https://youtu.be/e6WPY0Ngp2A?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Expressões Regulares: https://youtu.be/cRvy_gUEoQA?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Traduzindo a análise para um raspador: https://youtu.be/8b_S50gdKlg?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Indo além: https://youtu.be/gNbUQAicLAs?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Raspador para Petrópolis-RJ do início ao fim: https://youtu.be/s22_t4YTTTk?list=PLpWp6ibmzPTc2rod9Hc822_3zMaq9G-qE
.. _Querido Diário, hoje eu tornei um diário oficial acessível: https://escoladedados.org/coda/coda2020/workshop-querido-diario/
.. _Entenda como analisar sites de diários oficiais para raspagem de dados: https://queridodiario.ok.org.br/blog/post/30
.. _Conheça os desafios de raspagem do Querido Diário: https://queridodiario.ok.org.br/blog/post/28
.. _Como funciona o robozinho do Serenata que baixa os diários oficiais?: https://jvanz.com/como-funciona-o-robozinho-do-serenata-que-baixa-os-diarios-oficiais.html
.. _Quero tornar Diários Oficiais acessíveis. Como começar?: https://www.anapaulagomes.me/pt-br/2020/10/quero-tornar-di%C3%A1rios-oficiais-acess%C3%ADveis.-como-come%C3%A7ar/
.. _Monitorando o governo de Araraquara/SP: https://lcsvillela.com/querido-diario-monitorando-governo-araraquara-com-scrapy.html
.. _Macapá-AP: https://peertube.lhc.net.br/w/74K6zoanrm95R35tqKb44h
.. _Aracaju-SE: https://peertube.lhc.net.br/w/nY8cYyEVTjqqT1hy9BDbw1
.. _Itaúna-MG: https://peertube.lhc.net.br/w/6uzpyod2Z5a2GdfZWSCn4U
.. _Aracaju-SE + Maceió-AL: https://peertube.lhc.net.br/w/ceh11bzfF8x3PkzUdiVRDb
.. _Piracicaba-SP: https://peertube.lhc.net.br/w/8nrx1XbbLqaXnmwkKS6fuR
.. _Niterói-RJ: https://peertube.lhc.net.br/w/4xk5AJKTNsscFwqE8oiikw
.. _João Pessoa-PB: https://peertube.lhc.net.br/w/cQcKe99AQjUcRwbqBsTJCJ
.. _Teresina-PI: https://peertube.lhc.net.br/w/pSPvY6PjYd13QTf4VUzbFM
.. _Boa Vista-RR: https://peertube.lhc.net.br/w/v4CQXWUqE9QJ5BvuN7BQZc
.. _Maceió-AL: https://peertube.lhc.net.br/w/pg7XjLaHNP35YCcK4cHYgY
.. _Feira de Santana-BA: https://peertube.lhc.net.br/w/ehFgbkfnXMooc1MeeB1ndE
.. _Aldeias Altas-MA: https://peertube.lhc.net.br/w/6zHfRFyRnL75yybUskmhWx