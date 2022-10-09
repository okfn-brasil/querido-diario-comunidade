Arquitetura do projeto
======================

O projeto conta com diversos sistemas para que o diário publicado 
no site do órgão publicador chegue aqui de forma aberta e que torne 
acessível a interação com essa base de dados. A seguir temos itens 
responsáveis por cada parte do projeto.

Processamento de dados
----------------------

Esta etapa é essencial pois é aqui que acontece a mágica de transformar dados 
fechados em abertos e conectar outras etapas do nosso fluxo de dados. `No repositório`_ 
você encontrará várias tecnologias como: `Apache Tika`_ para extrair texto de documentos
de vários formatos, `elasticsearch`_ para interagir com o indexador de texto dos 
documentos, `postgresql`_ para interagir com os dados extraídos das spiders e muito 
Python puro.

Coleta de dados
---------------

Para entender o Querido Diário, é muito importante saber como funciona a 
estrutura de coleta de arquivos de diários oficiais. Todos os scripts são 
desenvolvidos utilizando o framework Scrapy, ferramenta muito popular para 
raspagem de dados. Caso nunca tenha utilizado, o tutorial é um ótimo caminho 
para aprender a fazer seus primeiros scripts de coleta de dados. Temos um 
repositório direcionado para organizar a coleta de arquivos. Nele, você 
encontrará arquivos em Python desenvolvidos pela comunidade para cada 
município, chamamos esses arquivos de spiders (Spider é o termo utilizado 
para um script de raspagem de dados no Scrapy).

Censo
-----

Antes de alguém conseguir escrever um spider para um município é importante 
sabermos em qual site os arquivos do diário oficial estão sendo publicados, 
e isso não é tão simples. Estamos falando de um país com 5.570 municípios 
que publicam seus atos públicos sem nenhum padrão, dentre eles alguns 
publicam em sites de associações, empresas privadas ou até mesmo site 
de outros entes da federação, e tudo isso sem nenhuma gestão de fluxo da 
informação.

Por isso, nós criamos uma iniciativa de mapeamento das fontes de publicação,
o `Censo Querido Diário`_, onde qualquer pessoa pode preencher um formulário 
e nos enviar a fonte de publicação de diários oficiais de um município.

Todas as fontes de publicação enviadas são validadas por um time voluntário 
e estão sendo feitas análises de dados que irão compor relatórios sobre o 
diagnóstico de como está esse retrato de divulgação de atos públicos no 
Brasil. Com acesso à base de dados você consegue utilizar a linguagem R e
Python para realizar análises exploratórias e contribuir no nosso 
`repositório do censo`_.

Toolbox
-------

O objetivo do `Querido Diário Toolbox`_ é fornecer para a comunidade um 
conjunto de ferramentas que possibilitem a análise e manipulação dos dados 
extraídos de diários oficiais. Com o toolbox, ampliamos as possibilidades 
de novos projetos que se utilizem da base de dados e compartilhamos 
conhecimento acumulado na limpeza e tratamento de dados que o projeto já 
enfrentou. O toolbox, inclusive, é utilizado internamente pela equipe da 
OKBR para o tratamento dos diários que são oferecidos na API; ele permite 
a extração de dados, metadados, limpeza de caracteres irrelevantes e 
extração de CPF e CNPJ. O toolbox não é, nem nunca será, um projeto 
concluído: com o apoio da comunidade, vamos sempre adicionar novas 
ferramentas e disponibilizá-las a todas as pessoas interessadas no projeto!

API pública
-----------

A `API pública`_ é uma das camadas principais do projeto, pois permite a 
divulgação da base de dados de uma forma acessível e automatizável por 
máquinas, além de promover um acesso direto aos dados para pessoas 
desenvolvedoras e pesquisadoras. Além disso, a plataforma consome os 
dados da API para a disponibilização dos dados para o público geral.

A API foi construída em Python com FastAPI e é integrada ao Swagger 
para a obtenção da documentação de forma automática. Você pode contribuir 
com ideias para a API no repositório oficial.

Front end 
---------

O front end é este que vos fala. Esta plataforma de busca é uma interface visual de 
interação com a API pública e também uma ferramenta de difusão de conhecimento e incentivo 
ao uso de diários oficiais.

O seu desenvolvimento é principalmente feito em Angular com Typescript, com responsividade 
para dispositivos móveis em primeiro lugar. Se te interessou contribuir, `o repositório oficial`_ 
também é aberto.


.. _No repositório: https://github.com/okfn-brasil/querido-diario-data-processing/
.. _Apache Tika: https://tika.apache.org/
.. _elasticsearch: https://www.elastic.co/
.. _postgresql: https://www.postgresql.org/ 
.. _Censo Querido Diário: https://censo.ok.org.br/
.. _repositório do censo: https://github.com/okfn-brasil/censo-querido-diario
.. _Querido Diário Toolbox: https://github.com/okfn-brasil/querido-diario-toolbox
.. _API pública: https://queridodiario.ok.org.br/api/docs
.. _FastAPI: https://fastapi.tiangolo.com/
.. _Swagger: https://swagger.io/
.. _repositório oficial: https://github.com/okfn-brasil/querido-diario-api
.. _o repositório oficial: https://github.com/okfn-brasil/querido-diario-frontend