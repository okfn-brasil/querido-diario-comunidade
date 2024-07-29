Arquitetura do Querido Diário
##############################

O Querido Diário lida com diferentes desafios -- descritos em :ref:`Enfrentando o 
deserto de dados` -- e emprega recursos diversos para superar cada um deles, a fim
de concretizar os :ref:`Pilares<Pilares: os valores do Querido Diário>` do projeto. 

Sua arquitetura é, portanto, reflexo das decisões técnicas que conectam essas duas 
pontas: o conjunto de soluções necessárias para enfrentar os obstáculos impostos 
pela disponibilização de diários oficiais e o interesse pela abertura destes dados. 

Podemos resumir o fluxo do projeto para cada um dos tipos de diário oficial às
etapas abaixo e compreendê-los mais detalhadamente a seguir. 

1. **Coletar**: obter arquivos de diários oficiais na fonte, os *sites* publicadores
2. **Processar**: aplicar tratamentos sobre os arquivos originais obtidos
3. **Disponibilizar**: permitir acesso e pesquisa nos conteúdos armazenados 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-dataflow-per-gazettes_ptbr.png
    :alt: [TODO]

1. Coleta de dados
**********************

Aqui, robôs raspadores são criados para, periodicamente, coletar dos *sites* publicadores
os arquivos de diários oficiais e metadados associados a eles, como data de publicação
e número da edição. Independente do tipo de diário oficial, todos eles demandam 
um raspador para coleta de arquivos. 

- **Desenvolvimento**: `Python`_ utilizando o *framework* `Scrapy`_. 
- **Armazenamento em produção**: DO Spaces para arquivos e `PostgreSQL`_ para metadados.
- **Repositório de código**: https://github.com/okfn-brasil/querido-diario
- **Agendamento de coletas**:
    - Série histórica disponível no *site*: uma vez, ao integrar o município ao projeto.
    - Edição mais recente: todos os dias, às 18h, no horário de Brasília.
    - Mês anterior (redundância): todo dia 1º, às 20h, no horário de Brasília.
- **Saiba mais**: :doc:`Documentação sobre Raspadores<../contribuindo/raspadores>`

2. Processamento de dados
****************************

Este é o coração do Querido Diário, a etapa responsável pelas operações em texto
e pela conexão entre os demais componentes da arquitetura. 

Aqui, o texto puro é extraído dos diários oficiais, salvo em arquivos de formato 
aberto TXT junto ao arquivo original, e tem seu conteúdo e metadados indexados
no motor de busca para viabilizar consultas aos textos.

.. note::
    Ainda que PDF Texto seja o formato de disponibilização mais comum por 
    parte dos municípios, esta etapa é capaz de extrair texto de arquivos em formato 
    DOC e DOCX, também.

Também é nesta etapa onde é implementada a integração de diários agregados por meio de
segmentadores. Estes são instrumentos que fatiam os trechos 
referentes a cada município, dos vários presentes no conteúdo do arquivo, e produzem
arquivos TXT individuais para cada município com os trechos.

Por fim, é realizada a filtragem temática, onde são realizadas buscas no índice de
principal dos diários para então criar índices com excertos associados a temas
específicos como "Tecnologias na Educação" e "Políticas Ambientais".

.. warning::
    No momento, o projeto não integra casos em que o formato do arquivo é PDF Imagem 
    pela baixa qualidade na extração de texto de imagens (OCR). Assim como diários 
    fragmentados por limitações na modelagem do banco de dados.

- **Desenvolvimento**: `Python`_, `Apache Tika`_ para extração textual e `OpenSearch`_ para filtros temáticos.
- **Armazenamento em produção**: DO Spaces para arquivos, `PostgreSQL`_ para metadados e `OpenSearch`_ como motor de busca.
- **Repositório de código**: https://github.com/okfn-brasil/querido-diario-data-processing

3. Disponibilização de dados
*********************************

API 
=====

A API Pública é a porta pela qual o acesso aos dados do projeto é facilitado. Ela
conta com diversos *endpoints* (pontos de acesso) pelos quais diferentes dados podem 
ser solicitados às bases de dados de diários oficiais e de `CNPJs da Receita Federal`_. 

- **Acesse**: https://queridodiario.ok.org.br/api/docs
- **Desenvolvimento**: `Python`_, biblioteca `FastAPI`_ e documentação automática com `Swagger`_.
- **Armazenamento em produção**: `PostgreSQL`_ para metadados de empresas e `OpenSearch`_ para buscas em diários.
- **Repositório de código**: https://github.com/okfn-brasil/querido-diario-api
- **Saiba mais**: :doc:`Recursos da API Pública<../utilizando/api-publica>`


Backend
========

O *Backend* é responsável por ser o mecanismo intermediador entre o frontend do Querido
Diário e sua API pública quando necessário. Conforme pessoas usuárias navegam o 
*site*, algumas de suas ações engatilham este mecanismo a solicitar dados à API Pública,
realizar operações e entregar os resultados ao *site*.

- **Desenvolvimento**: `Python`_, `Django REST Framework`_ e gestão de tarefas assíncronas com `Celery`_.
- **Armazenamento em produção**: `PostgreSQL`_ para gestão de usuário e alertas e `Redis`_ para tarefas assíncronas.
- **Repositório de código**: https://github.com/okfn-brasil/querido-diario-backend

Frontend
=========

É o *site* oficial do Querido Diário, sua interface visual para pessoas usuárias via 
navegação pela *Internet*. Nele, fica a interface de busca de diários oficiais. 
O *site* serve também como ferramenta de difusão do projeto: registra sua história 
e as pessoas que fazem parte dela, além da produção de conhecimento ao redor dele 
como análises, reportagens e pesquisas. 

- **Acesse**: https://queridodiario.ok.org.br
- **Desenvolvimento**: `TypeScript`_ e *framework* `Angular`_
- **Repositório de código**: https://github.com/okfn-brasil/querido-diario-frontend
- **Saiba mais**: :doc:`Recursos da Interface de Busca<../utilizando/interface-de-busca>`


Suporte 
**********

A documentação técnica oficial do projeto serve para auxiliar a compreensão de 
pessoas interessadas, usuárias e desenvolvedoras, registrando informações
fundamentais e mais profundamente descritas sobre como o Querido Diário funciona.

Por também cumprir esse papel de "conversa entre o projeto e seus contribuidores", no 
repositório de documentação fica o mapa de metas (:ref:`Roadmap`) do Querido Diário.

- **Desenvolvimento**: `Sphinx`_
- **Repositório de código**: https://github.com/okfn-brasil/comunidade
- **Saiba mais**: :doc:`Documentação sobre Documentação<../contribuindo/documentacao>`

Desenho da arquitetura
**************************

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-architecture_ptbr.png
    :alt: [TODO]

.. Referências
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
.. _CNPJs da Receita Federal: https://github.com/okfn-brasil/receita
