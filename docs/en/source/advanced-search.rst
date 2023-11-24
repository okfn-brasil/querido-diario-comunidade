Advanced Search in Gazettes
===========================

The `simple query string`_ syntax of Elasticsearch was implemented to enhance the search in the textual database of Official Gazettes. This mechanism uses characters as search operators to improve search results.

**When performing a search, each document in the publication database is checked, and it is returned if it matches the desired criteria. However, only the best excerpt of the document is shown as a result. This excerpt may not contain the complete search due to display space limitations. With more complex search operators, the excerpt may not be displayed as it becomes unclear which would be the best part to show. In any of these cases, the Official Gazettes listed as search results are correct.**

Learn about advanced search operators, their effects, and some examples:

\"OR\" Operator
---------------

It works by using the vertical bar symbol ( `|` ) to search for one term or another throughout the Gazette's content. Try view results for a search for `despacho | dispensa`_ and notice how the results include excerpts containing at least one of the words in their content.

*Important: This is the default operator in Querido Diário. If you don't specify which operator you want to use, the tool will always adopt it.*

\"AND\" Operator
----------------

It works by using the plus symbol ( `+` ) to search for one term and another throughout the Gazette's content. See this example search for `compra + computadores`_, whose results are Gazettes containing both terms.


Negation Operator
-----------------

It is a search command that uses the symbols `+-` to negate the term on the right. Searching for **ivermectina +-pandemia** is searching for Gazettes that contain the term \"ivermectina\" and, additionally, do not contain the term \"pandemia\" throughout their content.
Check out this search with the `negation operator`_.

*Note: To perform the search correctly, do not add a space between the negation symbol (-) and the negated term.*

Exact Match Operator
--------------------

This search works with a phrase in quotation marks (`\" \"`) for an exact search of the content, as in **\"lei de acesso à informação\"**. Check out this example of an `exact search`_ and notice that what was searched for corresponds exactly to what is in quotation marks, the same words in the same order.

*Note*: Watch out the importance of using quotation marks, as the search format without quotes also works in the project. If your search is **lei de acesso à informação** (without quotes), in practice, what is being searched is: lei (or) de (or) acesso (or) à (or) informação. See this example of a search `without quotes`_ and compare how its results are different from the exact search.

Prefix Operator
---------------

This operation uses the asterisk symbol ( * ) to search for a prefix. It is useful when the goal is to find words derived from the same root. Check out how the search for `democr*`_ brings results with democracia, democrático, democrata, democratização, etc.

Term Edit Operator
------------------

It works using the tilde symbol followed by a number (~N) for the term edit distance, i.e., the limit of character modifications a word needs to become another. An example is the search for `assento~3`_ that includes terms like acento, assunto, assentado; all of them differing by up to 3 changes from the searched word.

*Note: Another way to understand this search in the context of Gazettes is to think about typing errors since they are produced by people: the intention was to write a certain word, but another, very similar, came out.*

Term Distance Operator
----------------------

It works using a phrase in quotes followed by a tilde and a value (\" \"~N) indicating the maximum distance between words in the phrase. What will be searched for are Gazettes that have the terms in quotes close to each other, with up to N words separating them. When searching for `\"vazamento dados\"~50`_, what is being searched for are excerpts from Gazettes that have these two words separated by at most 50 other words.

**Tip**: The search for the maximum distance between words is especially interesting in the context of Querido Diário: it ensures that the searched content is close together and not scattered throughout the Gazette's text.

*Note: Note that the ~N operators serve for two types of searches: when associated with only one term or when associated with a phrase in quotes, working in a completely different way. Pay attention to their use.*

Precedence Operator
-------------------
Parentheses as operators indicate precedence and are used to force the search order. In general, they only make sense when the search becomes more complex, combining other operators.

You can check this search for `balanço + (financeiro | orçamentário)`_. By adding parentheses, the search is forced to happen in a certain order: first, it executes the command within the parentheses and then proceeds to execute the rest. In this case, it searches for the terms \"financeiro\" or \"orçamentário\" first, and from its result, it selects only the cases that also have \"balanço.\"

.. _simple query string: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-simple-query-string-query.html
.. _despacho | dispensa: https://queridodiario.ok.org.br/pesquisa?term=despacho%20%7C%20dispensa&since=2022-01-01&until=2022-07-31
.. _compra + computadores: https://queridodiario.ok.org.br/pesquisa?term=compra%20%2B%20computadores&since=2022-01-01&until=2022-07-31
.. _negation operator: https://queridodiario.ok.org.br/pesquisa?term=ivermectina%20%2B-pandemia&since=2022-01-01&until=2022-07-31
.. _exact search: https://queridodiario.ok.org.br/pesquisa?term=%22lei%20de%20acesso%20a%20informa%C3%A7%C3%A3o%22&since=2022-01-01&until=2022-07-31
.. _without quotes: https://queridodiario.ok.org.br/pesquisa?term=lei%20de%20acesso%20a%20informa%C3%A7%C3%A3o&since=2022-01-01&until=2022-07-31
.. _democr*: https://queridodiario.ok.org.br/pesquisa?term=democr*&since=2022-01-01&until=2022-07-31
.. _assento~3: https://queridodiario.ok.org.br/pesquisa?term=assento~3&since=2022-01-01&until=2022-07
.. _\"vazamento dados\"~50: https://queridodiario.ok.org.br/pesquisa?term=%22vazamento%20dados%22~50&since=2022-01-01&until=2022-07-31 
.. _balanço + (financeiro | orçamentário): https://queridodiario.ok.org.br/pesquisa?term=balan%C3%A7o%20%2B%20(financeiro%20%7C%20or%C3%A7ament%C3%A1rio)&since=2022-01-01&until=2022-07-31