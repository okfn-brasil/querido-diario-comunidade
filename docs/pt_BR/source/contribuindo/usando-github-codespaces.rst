.. este arquivo serve de aporte para o arquivo de Raspadores, mas não está referenciado na toctree

:orphan: 

GitHub Codespaces
###################

O `GitHub Codespaces`_ é um ambiente de desenvolvimento hospedado em nuvem oferecido
pelo GitHub. Por meio dele, você usa uma máquina virtual Docker contendo uma imagem 
Ubuntu Linux que inclui uma seleção de linguagens e ferramentas populares. Um *codespace* 
é acessado por meio do navegador e a interface de uso é a de um VSCode. Seu uso 
é gratuito dentro de um limite mensal.

Dos `benefícios do GitHub Codespaces`_, os mais relevantes para a comunidade do Querido
Diário, são:

    **Use um ambiente de desenvolvimento pré-configurado** - Você pode trabalhar 
    em um ambiente de desenvolvimento que foi configurado especificamente para o 
    repositório. Ele terá todas as ferramentas, linguagens e configurações necessárias 
    para trabalhar nesse projeto.

    **Acesse os recursos necessários** - O computador local pode não ter o poder 
    de processamento nem o espaço de armazenamento de que você precisa para trabalhar 
    em um projeto. O GitHub Codespaces permite que você trabalhe remotamente em um 
    computador com recursos adequados.

Usando um *codespace* para contribuir com raspadores
######################################################

1. Faça um *fork* do `repositório de raspadores`_.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/passo-1.png

2. No seu *fork*, crie um *codespace*.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/passo-2.png

3. Uma nova aba abrirá com seu *codespace* com visual de VSCode. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/passo-3.png

4. (opcional) Seu *codespace* é iniciado com um nome aleatório que pode ser modificado.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/passo-4.png

5. Visto que o *codespace* já é um ambiente virtual, os primeiros passos recomendados na `documentação do repositório`_ são dispensáveis. Por isso, basta seguir os passos 4 e 5 de instalação, executando os seguintes comandos no terminal do *codespace*:

.. code-block:: bash

    pip install -r data_collection/requirements-dev.txt
    pre-commit install

6. Pronto! Agora você já pode começar a contribuir!


Lembre-se: o *codespace* não está no seu computador
****************************************************

Uma das fases de desenvolvimento de raspadores é a revisão do código e a validação 
de execuções-testes. A execução do comando de coleta pode ser feito em um *codespace*,
mas tenha em vista que os arquivos coletados também estarão no *codespace*.

Para fazer a validação corretamente, pode ser necessário baixar arquivos do *codespace*
para seu computador. 

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/download-csv.png

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/documentacao-tecnica/usando-github-codespaces/download-pdf.png


.. REFERÊNCIAS
.. _GitHub Codespaces: https://docs.github.com/pt/codespaces
.. _benefícios do GitHub Codespaces: https://docs.github.com/pt/codespaces/overview#benef%C3%ADcios-do-github-codespaces
.. _repositório de raspadores: https://github.com/okfn-brasil/querido-diario
.. _documentação do repositório: https://github.com/trevineju/querido-diario/blob/main/docs/CONTRIBUTING.md#em-linux