Raspadores
############

Responsáveis pela etapa de coleta de dados na :doc:`../entendendo/arquitetura`
do Querido Diário, os raspadores são robôs programados para obter os arquivos de
diários oficiais e seus metadados nos *sites* publicadores. O principal desafio
é o de aumentar cada vez a cobertura, visando alcançar todos os 5570 municípios 
brasileiros. 

Entendendo o código 
************************

Os raspadores são desenvolvidos em `Python`_ utilizando o *framework* `Scrapy`_. Os
principais componentes de código do repositório se relacionam conforme o diagrama
enumerado abaixo. Cada um é mais aprofundado nos tópicos enumerados correspondentes 
a seguir e, ao fim, é apresentado a ordem em que esses componentes são mobilizados 
quando um comando de raspagem é acionado. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/scrapers-class-hierarchy_ptbr.png
    :alt: [TODO]

1. O framework Scrapy
========================

O *framework* de raspagem que o Querido Diário utiliza é o `Scrapy`_. Isso quer 
dizer que o Scrapy já oferece alguns recursos genéricos, restando ao Querido 
Diário a decisão de utilizá-los e/ou modificá-los para aplicação mais específica
que o projeto demande. 

Caso não conheça, a documentação oficial tem uma seção de primeiros passos, com 
páginas como `uma primeira olhada no Scrapy (ENG)`_ e `tutorial Scrapy (ENG)`_,
que indicamos. Para o Scrapy, cada *script* de raspagem se chama **spider**, título 
que aparecerá diversas vezes também no Querido Diário. 

2. A classe Gazette
=====================

Tendo em vista que coletar os arquivos de diários oficiais é o objetivo aqui, 
a classe `Gazette`_ (diário oficial em inglês) foi criada para vincular cada 
edição às suas informações complementares (metadados). Esta classe é criada a 
partir de `scrapy.Item`_ e é composta por atributos personalizados para os fins 
do Querido Diário. 

.. class:: Gazette (date, edition_number, is_extra_edition, power, file_urls )

   .. attribute:: date
   
      :tipo: `datetime.date()`_
      :definição: Data de publicação informada no *site* publicador de um diário oficial.
   
   .. attribute:: edition_number
   
      :tipo: **string**
      :definição: Número informado no *site* publicador de um diário oficial.
   
   .. attribute:: is_extra_edition
   
      :tipo: **boolean**
      :definição: Informação do *site* publicador se a edição de um diário oficial é extra. Costuma ser identificado pela presença de termos como "Extra", "Extraordinário", "Suplemento". Quando não informado, deve ser fixado em ``False`` por padrão (*hard coded*). 
   
   .. attribute:: power
   
      :tipo: **string**
      :opções: ``'executive'`` ou ``'executive_legislative'``
      :definição: Informação se o conteúdo do diário oficial é do poder executivo exclusivamente ou se aparecem atos oficiais do poder legislativo também.
   
      Atributo de preenchimento manual pela pessoa desenvolvedora (*hard coded*) a partir da consulta em alguns arquivos de diários oficias disponibilizados no *site* publicador.
       
   .. attribute:: file_urls
   
      :tipo: **list[string]**
      :definição: URL para *download* do arquivo correspondente a edição do diário oficial. Costuma ser apenas uma URL.


3. A classe BaseGazetteSpider
===============================

`BaseGazetteSpider`_ é a classe-mãe para todos os raspadores do Querido Diário. 
Ela é criada a partir de `scrapy.Spider`_, em uma relação de herança. Juntas, 
*scrapy.Spider* e **BaseGazetteSpider** criam uma lista de elementos obrigatórios
para os raspadores :class:`UFMunicipioSpider` implementarem. A procedência de cada
campo é indicada na definição da classe.

4. As classes UFMunicipioSpider
=================================

Para cada município integrado ao Querido Diário, uma classe :class:`UFMunicipioSpider`
correspondente deve existir no diretório de `spiders`_. Sua responsabilidade é a
de coletar as informações nos *sites* publicadores de diários oficiais do município
em questão. 

Estruturalmente, a :class:`UFMunicipioSpider` faz uma ponte entre as duas classes 
mencionadas acima: 

a. Ela é criada a partir de **BaseGazetteSpider**. Isso significa que herda 
a exigência de implementar elementos provindos dela.

b. Ela cumpre sua missão uma vez que tenha construíndo um objeto :class:`Gazette`, 
contendo todos os metadados listados no item 2.

.. class:: UFMunicipioSpider(BaseGazetteSpider)

   .. attribute:: name

      :procedência: `scrapy.Spider.name`_
      :tipo: **string**
      :definição: Nome do raspador no formato ``uf_nome_do_municipio``. Definido para ser usado no comando de execução da *spider*.
   
   .. attribute:: TERRITORY_ID

      :procedência: `BaseGazetteSpider`_
      :tipo: **string**
      :definição:  Código do IBGE para o município conforme listado no `arquivo de territórios`_.
   
   .. attribute:: allowed_domains
   
      :procedência: `scrapy.Spider.allowed_domains`_
      :tipo: **list[string]**
      :definição:  Domínios em que a *spider* está autorizada a navegar. Evita que a *spider* visite ou colete arquivos de outros *sites*.
       
   .. attribute:: start_urls

      :procedência: `scrapy.Spider.start_urls`_
      :tipo: **list[string]**
      :definição: URL da página onde ficam os diários oficiais no *site* publicador. É apenas uma URL e não deve ser a *homepage* do *site*.
      :exigência: Opcional. Saiba mais em :ref:`start-urls-ou-start-requests`
   
   .. attribute:: start_date

      :procedência: `BaseGazetteSpider`_
      :tipo: `datetime.date()`_      
      :definição: Data da primeira edição de diário oficial disponibilizado no *site* publicador.
   
   .. attribute:: end_date

      :procedência: `BaseGazetteSpider`_
      :tipo: `datetime.date()`_      
      :definição: Data da última edição de diário oficial disponibilizado no *site* publicador. 
      :exigência: Implícito. Por padrão, é preenchido automaticamente com a data da execução do raspador (``datetime.today().date()``). Só é explicitado em raspadores que coletam *sites* descontinuados, mas que seguem no ar por conservação de memória.
   
   .. method:: start_requests()
   
      :procedência: `scrapy.Spider.start_requests()`_
      :returns: `scrapy.Request()`_
      :definição: Método para criar URLs para as páginas de diários oficiais do *site* publicador
      :exigência: Opcional. Saiba mais em :ref:`start-urls-ou-start-requests`
   
   .. method:: parse(response)
   
      :procedência: `scrapy.Spider.parse()`_
      :returns: :class:`Gazette`
      :definição: Método que implementa a lógica de extração de metadados a partir do texto da `Response`_ obtida do *site* publicador. É o *callback* padrão.


Esqueleto de um UFMunicipioSpider
----------------------------------

Com isso, já é possível visualizar um esqueleto de todos os *scripts* de raspadores 
do Querido Diário e entender suas partes básicas. Note como todos os elementos
listados acima a seguir.

.. code-block:: python

    from gazette.items import Gazette
    from gazette.spiders.base import BaseGazetteSpider

    class UFMunicipioSpider(BaseGazetteSpider):  
        name
        TERRITORY_ID                                                      
        allowed_domains
        start_urls
        start_date                    

        def start_requests(): 

        def parse():

            yield Gazette(
                date  
                edition_number       
                is_extra_edition
                file_urls    
                power
                )


.. _classe-sistema:

5. As classes SistemaGazetteSpider
====================================

Em alguns casos, o *site* publicador de diários oficiais tem o mesmo *layout* entre 
diversas cidades. Isso parece acontecer quando municípios contratam a mesma solução
publicadora de diários ou de desenvolvimento de *sites*. Por exemplo, note como os 
*sites* de `Acajutiba (BA)`_, `Cícero Dantas (BA)`_ e `Monte Santo (BA)`_ tem a mesma
aparência.

A implicação disso para o Querido Diário é que o código das classes *BaAcajutibaSpider*,
*BaCiceroDantasSpider* e *BaMonteSantoSpider* seria enormemente parecido: seus 
raspadores "navegariam" nos *sites* da mesma maneira para obter metadados que ficam 
na mesma posição. 

Para simplificar a situação, enxugando repetição de código e facilitando a adição 
de novos raspadores a partir do padrão conhecido - *até porque estes são 3 casos,
quantos outros existem?* - temos as classes **SistemaGazetteSpider**. 

.. note::
    Adotamos a terminologia **sistema replicável** para nos referir ao padrão e 
    **município replicado** os municípios que o utilizam.

.. important::
    Vários padrões são conhecidos. Seus códigos estão no `diretório de bases`_ e 
    seus *layouts* na :doc:`Lista de Sistemas Replicáveis<lista-sistemas-replicaveis>`

Esqueleto de um SistemaGazetteSpider
--------------------------------------

Como parte da família de *spiders* do Querido Diário, a **SistemaGazetteSpider**
é criada a partir de **BaseGazetteSpider** e precisa, ao final, coletar **Gazettes**. 
Porém, cabe aqui o exercício de separar o que é recurso comum a diversos *sites* - 
para ficar no código de **SistemaGazetteSpider** - do que é específico de cada 
município - para ficar no código de :class:`UFMunicipioSpider`. 

De modo geral, o método :meth:`~UFMunicipioSpider.parse` responsável por navegar 
o *site* fica em **SistemaGazetteSpider** e os atributos :attr:`~UFMunicipioSpider.TERRITORY_ID`,
:attr:`~UFMunicipioSpider.name`, :attr:`~UFMunicipioSpider.start_date` e 
:attr:`~Gazette.power`, por serem dados particulares de cada município, tendem a
ficar em :class:`UFMunicipioSpider`.  

**SistemaGazetteSpider**

.. code-block:: python

    from gazette.items import Gazette
    from gazette.spiders.base import BaseGazetteSpider

    class SistemaGazetteSpider(BaseGazetteSpider):

        def parse():
            
            yield Gazette(
                date   
                edition_number        
                is_extra_edition
                file_urls      
            )

**UFMunicipioSpider para o padrão SistemaGazetteSpider**

.. code-block:: python

    from gazette.spiders.base.<sistema> import SistemaGazetteSpider

    class UFMunicipioSpider(SistemaGazetteSpider):
        TERRITORY_ID 
        name 
        start_date 
        power

.. attention::
    Perceba que os atributos :attr:`~UFMunicipioSpider.allowed_domains` e :attr:`~UFMunicipioSpider.start_urls` 
    e o método :meth:`~UFMunicipioSpider.start_requests()` não aparecem nos rascunhos. 
    Eles são os elementos mais influenciados pela situação, podendo ficar no código
    de *SistemaGazetteSpider* ou *UFMunicipioSpider* a depender do caso. 

6. Fluxo de execução 
======================

Idealmente, ao executar :ref:`o comando de raspagem<Executando um raspador>` para 
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

1. quando :class:`UFMunicipioSpider` tem seu próprio ``start_requests()``, não sendo usado o que existe em scrapy.Spider.
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
a URL muda a depender dos *endpoints* e seus campos. Outros diversos casos acontecem.

Nessas situações, a opção nativa não serve por ser muito restrita, uma vez que o 
quê ela espera receber é uma URL já fixada. 

Quando a situação demanda várias URLs e/ou parâmetros de contexto, a operação
padrão do método ``start_requests()`` deve ser sobreescrita com um :meth:`~UFMunicipioSpider.start_requests()` 
novo dentro do raspador que implemente uma lógica particular de construção dinâmica 
de URLs. Com :meth:`~UFMunicipioSpider.start_requests()` gerando URLs, o atributo
:attr:`~UFMunicipioSpider.start_urls` não tem porque existir. 

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
Para implementar um ``parse()`` que cumpra bem seu papel de obtenção de metadados a partir do conteúdo textual da `Response`_ 
do *site*, é importante que a pessoa desenvolvedora saiba inspecionar uma página 
*web* a fim de identificar seletores e construir `expressões regulares (RegEx)`_ 
convenientes para serem usados no método. 

O Scrapy conta com `seletores (ENG)`_ nativos que podem ser testados usando o 
`Scrapy shell`_. Já para expressões regulares, é comum o uso da biblioteca `re`_ 
de Python e, como sugestão, *sites* que testam *strings regex*, como `Pyregex`_, podem
ajudar.

Menos comuns, mas por vezes necessárias, outras bibliotecas já estão entre as dependências 
do repositório de raspadores e podem ser úteis: `dateparser`_, para tratar datas
em diferentes formatos, e `chompJS`_, para transformar objetos JavaScript em estruturas
Python.

.. tip::
    Materiais complementares são indicados na seção :ref:`Aprenda mais sobre raspagem` 

Quando o :meth:`~UFMunicipioSpider.parse()` não é o método de *callback* 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

O :meth:`~UFMunicipioSpider.parse()` só é o método padrão para o qual a `Response`_ é
enviada por ser assim que o método ``start_requests()`` nativo do Scrapy define. 
Porém, quando for o caso do :ref:`raspador implementar um start_requests() próprio<start-urls-ou-start-requests>`,
pode ser opção da pessoa desenvolvedora indicar outro método como *callback*. O 
`raspador para Macapá-AP`_ é um exemplo disso.

.. important::
  Um método nomeado como :meth:`~UFMunicipioSpider.parse()` pode não existir, mas 
  o papel que espera-se que ele cumpra segue necessário e deve ser realizado pelo 
  novo *callback* ou outros métodos auxiliares adicionados ao raspador. 


Contribuindo com raspadores  
*******************************

Passos iniciais
=================

Antes de colocar a mão na massa, é necessário configurar o ambiente de desenvolvimento
com as dependências que o repositório de raspadores precisa para funcionar e escolher
um município para o qual contribuir. Você achará informações sobre esses passos iniciais 
nas referências abaixo. Lembre-se de seguir o :doc:`guia-de-contribuicao` durante 
as interações no repositório. 

- **Repositório**: https://github.com/okfn-brasil/querido-diario
- **Configuração do Ambiente de Desenvolvimento**: `CONTRIBUTING`_
- **Mapeamento de municípios para contribuição**: `Quadro de Expansão de Cidades`_

.. attention::
  No momento, os únicos casos de raspadores sendo integrados são os para diários 
  completos ou diários agregados. Atente-se a isso antes de começar a desenvolver 
  e entenda mais dessa situação na seção sobre :ref:`tipos de diários oficiais<tipo-diarios>`

Diretrizes 
============

O desenvolvimento de raspadores é norteado pelo interesse de coletar **todos**
os diários oficiais disponíveis fazendo o **menor número de requisições possível**
ao *site* publicador, evitando correr o risco de sobrecarregá-lo. Queremos, também, 
que seja possível fazer coletas com períodos personalizados (uma semana, um mês, ...)
dentro da série histórica completa de disponibilização. 

Por isso, :class:`UFMunicipioSpider` tem os atributos :attr:`~UFMunicipioSpider.start_date` e 
:attr:`~UFMunicipioSpider.end_date` e seus métodos :meth:`~UFMunicipioSpider.parse()`
e/ou :meth:`~UFMunicipioSpider.start_requests()` devem ter, onde e como for conveniente,
filtros ou condicionais para controlar a data sendo raspada, mantendo-a dentro do
intervalo de interesse ao seguir executando. 

.. tip::
    Durante o desenvolvimento, para evitar fazer requisições repetidas nos *sites* 
    é possível utilizar a configuração `HTTPCACHE_ENABLED`_ do Scrapy. Isso também 
    faz com que as execuções sejam mais rápidas, já que todos os dados ficam armazenados 
    localmente.

Modelo para um raspador
=========================

Abaixo, temos um protótipo inicial e genérico para raspadores do Querido Diário. 
Eles acabam não sendo estritamente assim, uma vez que a depender da situação outros
atributos, métodos ou bibliotecas podem ser necessários ou, quando for o caso, pode 
precisar de uma :ref:`classe SistemaGazetteSpider<classe-sistema>` intermediária. 

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

        def start_requests(self):       # (caso necessário)
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

            yield Gazette(
                date = date(),  
                edition_number = "",       
                is_extra_edition = False,
                file_urls = [""],    
                power = "executive",
            )

Estratégias comuns
====================

Navegar pelo diretório de `spiders`_ lendo o código de raspadores existentes
ajudará o desenvolvimento de novos raspadores, principalmente com ideias para 
desafios frequentes. Abaixo, estão listadas mecânicas comuns que aparecem em *sites*
e referências de raspadores que implementaram uma solução. 

Paginação
-----------

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
-----------------

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
------------------

Quando é percebido que as requisições do *site* se dão por meio de APIs Públicas 
devolvendo um formato JSON. 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Caso com API
     - Raspador
   * - `Exemplo de acesso à API em Natal-RN`_
     - `rn_natal.py`_

Executando um raspador
=========================

Para executar um raspador, utilizamos o comando `crawl`_, cuja sintaxe e opções 
mais relevantes são apresentadas a seguir. Este comando é um dos `comandos 
padrão (ENG)`_ oferecidos pelo *framework* Scrapy e conhecer os demais também pode 
ajudar a desenvolver raspadores com mais facilidade.

O comando de raspagem deve ser executado no diretório ``/data_collection`` com o 
ambiente de desenvolvimento ativo. Nele, surgirá um arquivo SQLite ``queridodiario.db``
e um diretório ``/data``, organizado por :attr:`~UFMunicipioSpider.TERRITORY_ID` 
e :attr:`~Gazette.date`, onde ficam os arquivos de diários oficiais baixados.

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio [+ opções]

- ``-a start_date=AAAA-MM-DD`` define novo valor para :attr:`~UFMunicipioSpider.start_date`
- ``-a end_date=AAAA-MM-DD`` define novo valor para :attr:`~UFMunicipioSpider.end_date`
- ``-s LOG_FILE=nome_arquivo.log`` salva as mensagens de log em arquivo de texto
- ``-o nome_arquivo.csv`` salva a lista de diários oficiais e metadados coletados em arquivo tabular

Execuções exigidas
------------------------

Para garantir que o raspador implemente todos os recursos necessários, experimente 
algumas execuções-chave fazendo as validações indicadas na seção a seguir. Testes 
com essas execuções são feitos durante a revisão de uma contribuição.  

1. **Coleta última edição**. Veja a data da edição mais recente no *site* publicador
e execute a raspagem a partir dela. 

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio -a start_date=AAAA-MM-DD

2. **Coleta intervalo**. Escolha um intervalo como uma semana, algumas semanas, 
um mês ou alguns meses e execute a coleta desse intervalo arbitrário. 

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio -a start_date=AAAA-MM-DD -a end_date=AAAA-MM-DD

3. **Coleta completa**. Execute a coleta sem filtro por datas para obter toda a 
série histórica de edições no *site* publicador.  

.. code-block:: sh

    scrapy crawl uf_nome_do_municipio -s LOG_FILE=nome_arquivo.log -o nome_arquivo.csv

Com esses três casos, garante-se que o raspador funciona para as rotinas do projeto: 
ao integrar um novo município ao Querido Diário, o raspador é executado para obter
todos os diários oficiais existentes *(coleta completa)*. A partir disso, no dia-a-dia,
eles devem obter apenas a edição daquele dia *(coleta última edição)*. Por fim, para 
confirmar que nenhuma edição retardatária foi perdida, coletas de redundância são
feitas por precaução *(coleta intervalo)*. 

Verificando a execução do raspador
-------------------------------------

Diários oficiais coletados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Em ``/data_collection/data``, deve-se verificar a aderência dos arquivos baixados,
como: se são ``.pdf`` mesmo; se o conteúdo é de um diário oficial realmente; se 
a data dentro do arquivo corresponde à data do diretório em que o arquivo está.

Arquivos auxiliares 
^^^^^^^^^^^^^^^^^^^^^^^

* ``nome_arquivo.csv``

Veja se os campos de metadados (:attr:`~Gazette.date`, :attr:`~Gazette.edition_number`, 
:attr:`~Gazette.is_extra_edition`, :attr:`~Gazette.power` e :attr:`~Gazette.file_urls`) 
estão todos coerentemente preenchidos para todos os itens. Como o arquivo é uma tabela, 
é útil ordenar as colunas para fazer a conferência. Em particular, é importante 
prestar atenção se a primeira e a última edição estão dentro do intervalo de coleta 
definido em :attr:`~UFMunicipioSpider.start_date` e :attr:`~UFMunicipioSpider.end_date`
e se faltam certas datas ou números de edição que possam indicar que edições não 
foram coletadas.

* ``nome_arquivo.log``

Tem uma seção de dados e estatísticas do comportamento do raspador ao fim do arquivo.
Nela, verifique pela existência de erros (``log_count/ERROR``) e analise outras 
informações relevantes (como ``file_count``, ``retry/count``, ``downloader/request_count``). 
Havendo indícios de problemas, procure no texto do *log* pelas mensagens que podem 
ajudar a entender mais situação a fim de corrigí-las. 

Aprenda mais sobre raspagem
******************************

**> Vídeos**

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

**> Sessões de desenvolvimento ou revisão de raspadores gravadas**

  Ana Paula Gomes   
    - `Querido Diário, hoje eu tornei um diário oficial acessível`_
  Giulio Carvalho   
    - Coding Dojo: `Raspador para Petrópolis-RJ do início ao fim`_
  Renne Rocha   
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

**> Textos**

  - Giulio Carvalho, `Entenda como analisar *sites* de diários oficiais para raspagem de dados`_
  - Juliana Trevine, `Conheça os desafios de raspagem do Querido Diário`_
  - Ana Paula Gomes, `Quero tornar Diários Oficiais acessíveis. Como começar?`_
  - Lucas Villela, `Monitorando o governo de Araraquara/SP`_
  - José Vanz, `Como funciona o robozinho do Serenata que baixa os diários oficiais?`_

.. Referências
.. _Python: https://www.python.org/
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
.. _Gazette: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/items.py
.. _scrapy.Item: https://docs.scrapy.org/en/latest/topics/items.html
.. _spiders: https://github.com/okfn-brasil/querido-diario/tree/main/data_collection/gazette/spiders
.. _Acajutiba (BA): https://doem.org.br/ba/acajutiba/diarios
.. _Cícero Dantas (BA): https://doem.org.br/ba/cicerodantas/diarios
.. _Monte Santo (BA): https://doem.org.br/ba/montesanto/diarios
.. _sistema replicável DOEM: https://github.com/okfn-brasil/querido-diario/edit/main/data_collection/gazette/spiders/base/doem.py
.. _diretório de bases: https://github.com/okfn-brasil/querido-diario/tree/main/data_collection/gazette/spiders/base
.. _CONTRIBUTING: https://github.com/okfn-brasil/querido-diario/blob/main/docs/CONTRIBUTING.md#como-configurar-o-ambiente-de-desenvolvimento
.. _datetime.date(): https://docs.python.org/3/library/datetime.html#datetime.date
.. _arquivo de territórios: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/resources/territories.csv
.. _scrapy.Request(): https://docs.scrapy.org/en/latest/topics/request-response.html#scrapy.http.Request
.. _Quadro de Expansão de Cidades: https://github.com/orgs/okfn-brasil/projects/12
.. _raspador para Paulínia-SP: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/sp/sp_paulinia.py
.. _raspador para Barreiras-BA: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ba/ba_barreiras.py
.. _raspador para Macapá-AP: https://github.com/okfn-brasil/querido-diario/blob/main/data_collection/gazette/spiders/ap/ap_macapa.py
.. _shell: https://docs.scrapy.org/en/latest/topics/shell.html
.. _HTTPCACHE_ENABLED: https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-enabled
.. _motor do Scrapy: https://docs.scrapy.org/en/latest/topics/architecture.html
.. _expressões regulares (RegEx): https://pt.wikipedia.org/wiki/Express%C3%A3o_regular
.. _seletores (ENG): https://docs.scrapy.org/en/latest/topics/selectors.html
.. _Pyregex: http://www.pyregex.com/
.. _re: https://docs.python.org/3/library/re.html
.. _chompJS: https://github.com/Nykakin/chompjs
.. _dateparser: https://github.com/scrapinghub/dateparser
.. _Scrapy shell: https://docs.scrapy.org/en/latest/topics/shell.html
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
.. _Entenda como analisar *sites* de diários oficiais para raspagem de dados: https://queridodiario.ok.org.br/blog/post/30
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
.. _comandos padrão (ENG): https://docs.scrapy.org/en/latest/topics/commands.html#available-tool-commands
.. _crawl: https://docs.scrapy.org/en/latest/topics/commands.html#crawl
.. _Response: https://docs.scrapy.org/en/latest/topics/request-response.html#response-objects
