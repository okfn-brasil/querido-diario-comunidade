Busca avançada nos diários
==========================

A sintaxe `simple query string`_ do Elasticsearch foi implementada para aperfeiçoamento de pesquisa na base de dados textuais dos Diários Oficiais. Este mecanismo funciona com o uso de caracteres como operadores de busca a fim de aprimorar os resultados.

**Ao executar uma busca, cada documento da base de publicações é verificado e retornado caso corresponda com o critério desejado. Porém, apenas o melhor trecho (ou excerto) do documento é mostrado como resultado. Este excerto pode não conter a busca completa por limitação do espaço de exibição. Com operadores de busca mais complexos, o excerto pode não ser nem exibido já que deixa de ficar claro qual seria o melhor trecho para mostrar. Em qualquer um desses casos, os Diários Oficiais listados como resultados da busca estão corretos.**

Conheça operadores da busca avançada, seus efeitos e alguns exemplos:


Operador "OU"
-------------

Também conhecido como “operador OR”, funciona usando o símbolo de barra vertical ( | ) para buscar um termo ou outro em toda extensão do diário. Experimente `ver os resultados da busca por despacho | dispensa`_ e repare como são de trechos que tem pelo menos uma das palavras em seu conteúdo.

*Atenção:* Este é o operador padrão no Querido Diário. Se você não explicitar qual operador quer utilizar, a ferramenta vai sempre adotá-lo.


Operador "E"
------------

Também conhecido como “operador AND”, funciona usando o símbolo de adição ( + ) para buscar um termo e outro em toda a extensão do diário. Veja este exemplo de busca por `compra + computadores`_, cujos resultados são de diários que contêm ambos termos.


Operador de Negação
-------------------

É um comando de busca que usa os símbolos +- para negar o termo à direita. Buscar por **ivermectina +-pandemia** é buscar diários que contém o termo “ivermectina” e, adicionalmente, não contém o termo “pandemia” por toda sua extensão. 
`Confira esta busca com o operador de negação`_.

*Observação:* para a busca funcionar corretamente, não devemos adicionar um espaço entre o símbolo de negação (-) e o termo negado.


Operador de Busca Exata
-----------------------

Esta busca funciona com uma frase entre aspas (“ ”) para busca exata do conteúdo, como em **“lei de acesso à informação”**. Verifique `este exemplo de busca exata`_ e repare que o que foi buscado corresponde exatamente ao que está entre aspas, as mesmas palavras na mesma ordem.

*Observação:* note a importância de se utilizar as aspas, já que o formato de busca sem aspas também funciona no projeto. Se sua pesquisa for **lei de acesso à informação** (sem aspas), na prática, o que está sendo buscado é: lei (ou) de (ou) acesso (ou) à (ou) informação. Veja `este exemplo de busca sem aspas`_ e compare como os resultados dela são diferentes da busca exata.


Operador de Busca por Prefixo
-----------------------------

Esta operação utiliza o símbolo de asterisco ( * ) para buscar por prefixo. Serve para quando o objetivo é achar palavras derivadas de um mesmo radical. Confira `como a pesquisa por democr*`_ traz resultados com democracia, democrático, democrata, democratização, etc.


Operador de Edição de Termo
---------------------------

Funciona utilizando o símbolo til seguido por um número (~N) para distância de edição de termo, ou seja, qual o limite de modificações de caracteres uma palavra precisa para se transformar em outra. Um `exemplo é a pesquisa por assento~3`_ que inclue termos como *acento, assunto, assentado*; todos eles distando até 3 alterações da palavra buscada.

*Observação: Outra forma de entender essa busca no contexto dos diários é pensar em erros de digitação já que são produzidos por pessoas: a intenção era escrever certa palavra, mas acabou saindo outra, muito próxima.*


Operador de Distância entre termos
----------------------------------

Funciona usando uma frase entre aspas seguida de um til e um valor (“ “~N) indicando a busca como distância máxima entre palavras da frase. O que será buscado são diários que têm os termos entre aspas próximos entre si até N palavras. Ao buscar por “`vazamento dados`_”~50 o que está sendo buscado são trechos de diários que tenham essas duas palavras separadas, por no máximo, 50 outras palavras.
Dica: A busca por distância máxima entre palavras é especialmente interessante no contexto do Querido Diário: ela garante que o conteúdo buscado esteja próximo e não disperso por todo o texto do diário.

*Observação: note que os operadores ~N servem para dois tipos de busca: quando associados a apenas um termo ou quando estão associados a uma frase entre aspas, funcionando de forma completamente diferente. Tenha atenção em seu uso.*


Operador de Precedência
-----------------------

Os parênteses como operadores indicam precedência e são usados para forçar a ordem da busca. No geral, só fazem sentido quando a busca a ser feita se complexifica, combinando outros operadores.

Você pode conferir `esta busca por balanço + (financeiro | orçamentário)`_. Ao adicionar os parênteses, a busca é forçada a acontecer em certa ordem: primeiro executa o comando entre parênteses e então passa a executar o resto. Neste caso, busca pelos termos “financeiro” ou “orçamentário” primeiro e, de seu resultado, seleciona apenas os casos que também tem “balanço”.


.. _simple query string: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-simple-query-string-query.html
.. _ver os resultados da busca por despacho | dispensa: https://queridodiario.ok.org.br/pesquisa?term=despacho%20%7C%20dispensa&since=2022-01-01&until=2022-07-31
.. _compra + computadores: https://queridodiario.ok.org.br/pesquisa?term=compra%20%2B%20computadores&since=2022-01-01&until=2022-07-31
.. _Confira esta busca com o operador de negação: https://queridodiario.ok.org.br/pesquisa?term=ivermectina%20%2B-pandemia&since=2022-01-01&until=2022-07-31
.. _este exemplo de busca exata: https://queridodiario.ok.org.br/pesquisa?term=%22lei%20de%20acesso%20a%20informa%C3%A7%C3%A3o%22&since=2022-01-01&until=2022-07-31
.. _este exemplo de busca sem aspas: https://queridodiario.ok.org.br/pesquisa?term=lei%20de%20acesso%20a%20informa%C3%A7%C3%A3o&since=2022-01-01&until=2022-07-31
.. _como a pesquisa por democr*: https://queridodiario.ok.org.br/pesquisa?term=democr*&since=2022-01-01&until=2022-07-31
.. _exemplo é a pesquisa por assento~3: https://queridodiario.ok.org.br/pesquisa?term=assento~3&since=2022-01-01&until=2022-07-31
.. _vazamento dados: https://queridodiario.ok.org.br/pesquisa?term=%22vazamento%20dados%22~50&since=2022-01-01&until=2022-07-31 
.. _esta busca por balanço + (financeiro | orçamentário): https://queridodiario.ok.org.br/pesquisa?term=balan%C3%A7o%20%2B%20(financeiro%20%7C%20or%C3%A7ament%C3%A1rio)&since=2022-01-01&until=2022-07-31
