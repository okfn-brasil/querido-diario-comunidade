Configuração de ponta-a-ponta
#############################

Às vezes é necessário integrar diversas etapas do Querido Diário, principalmente ao
desenvolver soluções no frontend que necessitam de dados que ainda não estão em
produção.

Como o QD utiliza `podman`_ para gerir sua infraestrutura, orquestrar as etapas
consiste em ajustar variáveis de ambiente e garantir que os containers estão
conseguindo se comunicar.

.. attention::
    Esta documentação foi testada principalmente em ambientes Linux. Ambientes
    Windows WSL ou MacOS podem ter algumas diferenças (contribuições são bem vindas!).

Começando a configurar pelo processamento de dados
**************************************************

Como os recursos (banco de dados, motor de busca, etc.) utilizados pelo QD localmente
serão compartilhados por diferentes etapas, o `processamento de dados`_ é um bom
projeto para configurar inicialmente, pois utiliza diversos desses recursos em suas
tarefas de processamento.

Para configurá-lo, seguiremos o passo-a-passo:

1. `Instale o podman`_ (caso não esteja instalado);

2. Execute:

    .. code-block:: bash

      make build

3. Execute (``FULL_PROJECT=true`` faz com que portas adicionais sejam abertas pelo pod
   que será criado):

    .. code-block:: bash

      FULL_PROJECT=true make setup

.. tip::
    Se essa for a primeira vez que está executando o projeto, as variáveis de ambiente
    serão criadas no arquivo ``envvars``, na raíz do repositório. Aqui você terá as
    credenciais do armazenamento de objetos (Minio), por exemplo.

.. warning::
    Ao executar o comando ``make setup``, um erro ``bind: address already in use`` pode
    aparecer. Nesse caso, verifique se não tem algum outro serviço no seu computador
    utilizando a porta indicada e o pare (ou modifique as variáveis de ambiente e/ou
    Makefile, se souber o que está fazendo).

    Caso a porta esteja sendo usada pelo próprio podman por algum erro de execução você
    pode terminar o programa com o comando (usando a porta 8000 de exemplo) ``sudo
    kill -9 $(sudo lsof -t -i:8000)``.

    Ao terminar, execute o ``make setup`` novamente.

Agora o pod foi criado, e dentro dele vários recursos como Opensearch, Postgres e Minio
estão sendo executados. Porém, eles ainda estão "vazios". Vamos populá-los utilizando
o repositório de raspadores.

Gerando dados com os raspadores
*******************************

Queremos executar raspadores para popular nosso banco com diários oficiais para serem
processados. Para isso, só precisamos configurar o seguinte no
`repositório de raspadores`_:

1. Copie ``data_collection/.local.env`` para ``data_collection/.env``;

2. Configure o ambiente de desenvolvimento e execute raspadores normalmente, como
   indicado em sua documentação.

Processando os documentos raspados
**********************************

Agora que temos o armazenamento de objetos e algumas tabelas do banco Postgres
populadas, vamos executar o pipeline principal do processamento de dados para que os
arquivos TXT sejam gerados e o motor de busca seja populado:

1. Execute, no repositório do processamento de dados:

    .. code-block:: bash

      make re-run

.. note::
    Perceba que o ``make re-run`` está sendo executado aqui e não o ``make run``, pois
    o ``make run`` executa o ``make setup``, e se o ``make setup`` for executado, todos
    os recursos serão destruídos e reconstruídos novamente, anulando a raspagem que foi
    realizada.

Agora temos arquivos, tabelas e índices populados. Podemos habilitar a API.

Habilitando a API
*****************

1. Execute, no repositório da `API`_:

    .. code-block:: bash

      make build

2. Execute:

    .. code-block:: bash

      make re-run

.. note::
    Pelo mesmo motivo que no processamento de dados, o ``make re-run`` está sendo
    executado aqui e não o ``make run``.

Com a API disponível, podemos iniciar o backend local.

Habilitando o backend
*********************

Para lidar com o `Querido Diário: Tecnologias na Educação`_, é necessário configurar o
`backend`_ da seguinte forma:

1. Configure o ambiente de desenvolvimento como indicado na documentação;

2. Faça o cadastro do superusuário como pedido.

Com o backend disponível, o frontend que usa API e backend locais também pode ser
configurado.

Habilitando o frontend
**********************

Finalmente chegamos na outra ponta da arquitetura do QD, o `frontend`_! Aqui vamos fazer o seguinte:

1. Configure o ambiente de desenvolvimento como indicado na documentação;

2. Aplique esse patch no repositório:

    .. code-block:: bash

      ./src/app/constants.ts
      - export const API = 'https://queridodiario.ok.org.br/api';
      + export const API = 'http://localhost:8080';

      ./src/app/services/utils/index.ts
      - export const educationApi = 'https://backend-api.queridodiario.ok.org.br/api/';

      + export const educationApi = 'http://localhost:8000/api/';

Pronto! Agora o ambiente está todo configurado 🎉

Dicas de uso do ambiente
************************

Algumas maneiras úteis de usar o ambiente de desenvolvimento:

1. Quer acessar o banco postgres para ver registros de diários oficiais, usuários do
backend, etc.?

    Execute ``make shell-database`` no repositório querido-diario-data-processing e
    estará na linha de comando ``psql``.

2. Quer acessar o motor de busca para ver os índices textuais de diários e excertos
temáticos?

    Execute:

    .. code-block:: bash

      curl -k -u admin:admin -X GET "localhost:9200/_cat/indices?v&pretty=true

    Outros endpoints funcionarão igualmente de acordo com a documentação do opensearch.

3. Quer acessar o sistema de arquivos para ver onde foram baixados?

    Acesse `localhost:9000 <http://localhost:9000>`_ com as credenciais localizadas no
    ``envvars`` do repositório querido-diario-data-processing.

4. Quer baixar mais arquivos de diários e processá-los?

    Execute outro ``scrapy crawl`` no repositório querido-diario e então execute ``make
    re-run`` no querido-diario-data-processing novamente.

5. No frontend o live reload está habilitado, mas na API e backend não. Como checar as
mudanças?

    Na API, execute ``make re-run`` novamente. No backend, execute:

    .. code-block:: bash

      python -m cli setup -- pod-name querido-diario

6. Como acessar a documentação da API?

    Acesse `0.0.0.0:8080/docs <0.0.0.0:8080/docs>`_.

    .. tip::
        Caso ``0.0.0.0`` não funcione, use `localhost:8080/docs
        <http://localhost:8080/docs>`_.

7. Como acessar o painel de admin do backend?

    Acesse `0.0.0.0:8000/api/admin <http://0.0.0.0:8000/api/admin>`_ com as credenciais
    de superusuário criadas anteriormente.

    .. tip::
        Caso ``0.0.0.0`` não funcione, use `localhost:8000/api/admin
        <http://localhost:8000/api/admin>`_.

.. Referências
.. _podman: https://podman.io/
.. _processamento de dados: https://github.com/okfn-brasil/querido-diario-data-processing/
.. _repositório de raspadores: https://github.com/okfn-brasil/querido-diario/
.. _API: https://github.com/okfn-brasil/querido-diario-api/
.. _backend: https://github.com/okfn-brasil/querido-diario-backend/
.. _frontend: https://github.com/okfn-brasil/querido-diario-frontend/
.. _Instale o podman: https://podman.io/docs/installation
.. _Querido Diário\: Tecnologias na Educação: https://queridodiario.ok.org.br/educacao

