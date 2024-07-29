Conheça o Querido Diário
###########################

Objetivos
************

O **diário oficial** é uma publicação feita pelas esferas da administração pública
brasileira, seja federal, estadual ou municipal e dos poderes executivo, legislativo
e judiciário, que serve para **tornar oficial para a população as ações tomadas pelos 
poderes**.

O Diário Oficial da União (DOU) e os Diários Oficiais dos Estados (DOE) são, frequentemente,
objetos de interesse coletivo já consolidados, enquanto os Diários Oficiais Municipais (DOM), 
especialmente de cidades que não fazem parte de uma região metropolitana, são menos 
acompanhados. Não à toa. Apesar de públicos, **esses documentos são disponibilizados
por vias difíceis de serem acompanhadas**.

O Querido Diário é o projeto que enfrenta esse deserto de dados, oferecendo **uma 
ferramenta que amplia o acesso à informação sobre a administração pública brasileira**
em sua mais local instância - os municípios -, através da abertura e centralização 
de diários oficiais eletrônicos. Não é uma empreitada fácil, sobretudo por existirem
**5570 municípios** no país e grandes discrepâncias quanto à existência e maturidade
na disponibilização *online* de seus dados e informações.

Pilares: os valores do Querido Diário 
****************************************

Entender o Querido Diário passa por conhecer onde o projeto quer chegar, quais seus
valores e recursos que almeja oferecer.

1. Informação pública contextualizada
=========================================

Adotamos o compromisso com disponibilizar diários oficiais sempre acompanhados 
de qual município diz respeito e sua data de publicação. Não devem ser integrados 
os casos em que algum desses dados não pode ser obtido. 

2. Disponibilização completa
===============================

O conteúdo dos diários oficiais, antes fechados em PDFs, deve ser transformado
em texto puro e armazenado em arquivo de formato aberto TXT. Ambos arquivos, o 
original e o aberto, são disponibilizados em sua completude pelo Querido Diário.

3. Enorme base de dados pesquisável
======================================

Ao adicionar cada vez mais municípios dos 5570 municípios brasileiros, queremos 
construir uma enorme e pioneira base de dados sobre a administração pública local 
para que a sociedade brasileira se beneficie dessas informações e, para isso, 
possibilitar amplos recursos de pesquisa em um ou vários municípios ao mesmo tempo. 

4. Acesso livre
===================

O acesso aos dados armazenados pelo Querido Diário e recursos de pesquisa nele 
implementados deve ser livre para pessoas usuárias de maneira amigável e pontual, 
e, para pessoas desenvolvedoras ou máquinas, de maneira programática.

Enfrentando o deserto de dados
*********************************

Para concretizar seus pilares, o Querido Diário demanda enfrentamento a diferentes 
situações, todas relacionadas às características da publicação de diários oficiais
municipais em um contexto onde não há legislação nacional que a padronize. Nessa
seção, qualificamos os aspectos mais relevantes e que configuram obstáculos sendo 
superados pelo projeto. 

1. Publicação eletrônica
===========================

Para o Querido Diário ter os diários oficiais de um município, este deve
publicar arquivos em algum *site*, publicamente na *Internet*. Um município que não 
tem *site* oficial ou não disponibiliza seus diários virtualmente não pode ser 
integrado ao projeto. 
 
2. Formato do arquivo
=======================

Quando falamos "obter diários oficiais", isso quer dizer, mais especificamente, 
o interesse pelo seu conteúdo: é o texto do decreto, da portaria, da licitação, 
etc, que queremos; eles são armazenados e compartilhados por meio de arquivos. 

Alguns formatos de arquivos são mais visuais e apropriados para leitura por seres 
humanos, enquanto outros priorizam o fácil acesso a seu conteúdo textual, sendo 
apropriados para leitura por máquinas. No geral, municípios priorizam a publicação 
para leitura humana e disponibilizam seus atos oficiais por meio de arquivos PDF 
dificultando o processamento de seu conteúdo de forma automática. Ainda, eles 
ser de dois tipos:

- **PDF de Texto**: o conteúdo do PDF é um texto, provavelmente gerado diretamente pelo programa editor de texto.
- **PDF de Imagem**: o conteúdo do PDF é uma imagem e o arquivo foi criado a partir do escaneamento da edição impressa.  

.. _tipo-diarios:

3. Tipo do diário oficial
===========================

Apesar da ausência legal de padrão, percebemos ser possível classificar os diários 
oficiais eletrônicos em 3 categorias: 

- **Diário agregado**: o documento contém atos oficiais de diversos municípios, normalmente por comporem uma Associação/Federação ou o Diário do Estado tem uma seção para municipalidades. Um caso é o `Diário Oficial da Associação de Municípios de Alagoas`_.
- **Diário completo**: todos os atos oficiais do documento correspondem a um único município, como é o `diário oficial de Recife (PE)`_.
- **Diário fragmentado**: não há um documento único para todos os atos oficiais do município, eles ficam pulverizados em vários arquivos. Um exemplo é `Bragança (PA)`_ que, em seu *site*, tem seções para decretos, portarias, etc, e nelas vários atos separados.

Note como esses três tipos de diários diferem quanto a granularidade do conteúdo 
e se relacionam conforme o diagrama a seguir.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-document-types_ptbr.png
    :alt: [TODO]

.. REFERÊNCIAS:
.. _Diário Oficial da Associação de Municípios de Alagoas: https://www.diariomunicipal.com.br/ama/
.. _diário oficial de Recife (PE): https://dome.recife.pe.gov.br/dome/
.. _Bragança (PA): https://braganca.pa.gov.br/decretos-2023/