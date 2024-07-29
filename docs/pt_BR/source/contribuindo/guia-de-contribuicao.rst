Guia de Contribuição
######################

Este guia serve para orientar pessoas em sua jornada com o Querido Diário. Ele traz 
os passos iniciais que devem ser tomados ao interagir em todos os repositórios e
espaços virtuais ou presenciais do projeto. 

Cada repositório tem particularidades descritas em sua própria documentação. Ao 
finalizar a leitura destas orientações gerais, leia também os arquivos `README.md`
e `CONTRIBUTING.md` do repositório onde for contribuir, uma vez que eles tem informações 
mais específicas sobre o ambiente de desenvolvimento, instalação, execução, metas,
a dinâmica de interação em *Pull Requests* (PR), etc. 

Código de conduta
********************

Para garantir que a nossa comunidade seja acolhedora para todas as pessoas, leia
e siga o :doc:`Código de Conduta<codigo-de-conduta>` válido para todos os espaços 
da comunidade. **Ao participar deste projeto, você concorda em aderir aos termos
nele especificados**.

Contribuindo
**************

Ecossistema do Querido Diário
================================

O Querido Diário possui vários repositórios, possibilitando contribuições onde houver
mais afinidade:

- :doc:`raspadores` - https://github.com/okfn-brasil/querido-diario
- Processamento de Dados - https://github.com/okfn-brasil/querido-diario-data-processing
- API - https://github.com/okfn-brasil/querido-diario-api
- Backend - https://github.com/okfn-brasil/querido-diario-backend
- Frontend - https://github.com/okfn-brasil/querido-diario-frontend
- :doc:`documentacao` - https://github.com/okfn-brasil/querido-diario-comunidade

Descobrindo com o que contribuir
====================================

Conhecer sobre `git`_ e como a plataforma `GitHub`_ funciona é um passo fundamental. 
Indicamos o `tutorial da Escola de Dados`_ caso você ainda não conheça ou tenha pouca
familiaridade.

Issues  
------------
Na aba **Issues** do repositório escolhido ficam as tarefas ou problemas específicos 
daquele repositório. Veja se uma te interessa.

- Quando a tarefa é indicada para iniciantes terá uma etiqueta com ``good first issue`` (boa primeira tarefa) e quando for uma tarefa importante, alinhada às metas do projeto, terá a etiqueta ``priority`` (prioridade).
- Ao encontrar uma que interessar, comente que fará a tarefa. Assim outras pessoas saberão que você já está trabalhando nela, podendo oferecer ajuda ou se dedicando a outras que ainda não tenham sido iniciadas.
- Caso a tarefa tenha um comentário anunciando o interesse de alguma pessoa em trabalhar nela, mas não tem um *pull request* vinculado e o comentário faz mais de 1 mês considere a *issue* disponível.
- Já tendo, ou não, escolhido uma *issue* para trabalhar, você pode também comentar informações ou dúvidas relevantes relacionadas a tarefa.

Milestones
---------------

Se não houver *issues* abertas ou nenhuma te interessar, veja as **Milestones** 
(metas) do repositório. Explorar os arquivos buscando encontrar espaços de melhorias 
que ajudem o repositório a atingir uma *milestone* é uma das mais excelentes formas 
de contribuição.

- Ao encontrar, abra uma *issue* descrevendo a possibilidade de melhoria percebida e como ela se relaciona a uma das *milestones*. Se você já for realizar a tarefa, mencione na mensagem.

Quadros
------------

Exibidos na aba *Projects*, **Quadros** são recursos que agrupam *issues* de um mesmo
tema de um ou mais repositórios em um lugar centralizado, possibilitando adicionar
navegar nas tarefas disponíveis com mais facilidade. Veja quais são os `Quadros 
do Querido Diário`_.

Roadmap
------------

O `roadmap`_ é um quadro especial que registra os rumos estratégicos que o Querido 
Diário tem no horizonte, os objetivos atualmente priorizados e seus progressos. 

- Caso deseje sugerir novas metas para o *roadmap*, crie uma *issue* no `repositório de comunidade`_ usando o modelo "Item para o Roadmap".

Comunidade online
----------------------

- Ingresse no `Discord da OKBR`_. Lá você poderá tirar dúvidas mais rápidas, interagir com as pessoas mantenedoras, perguntar como poderia ajudar e ainda conversar com outras pessoas que também contribuem. 
- Também no Discord, organizamos e promovemos atividades como *sprints*, reuniões periódicas, pareamentos, plantões, etc. Todas essas atividades são abertas e podem ser acompanhadas por meio desta `agenda pública`_.

Reunião Geral do Querido Diário
---------------------------------

Sempre na última quinta-feira do mês, às 18h, acontece, no Discord, a Reunião Geral
do Querido Diário, que conta com a presença da equipe de Inovação Cívica da OKBR, 
pessoas mantenedoras e contribuidoras. 

- Por um lado, esta reunião cumpre o papel alinhamento interno da comunidade quanto às diversas frentes de desenvolvimento que podem estar ocorrendo. E, por outro, serve de porta de entrada para pessoas interessadas, visto que é uma reunião de caráter panorâmico, permitindo conheçam as pessoas envolvidas e atividades acontecendo. 

Interagindo com a pessoa revisora 
====================================

- Tenha paciência e dê tempo para a pessoa revisora. Além de revisões de código serem complexas e demandarem, por si só, tempo e atenção, uma pessoa revisora pode ser, assim como você, outra contribuidora cedendo seu tempo pessoal quando possível ou ter outras prioridades antes de poder revisar seu código.
- Seja em forma de código ou com mensagens de dúvidas e sugestões, facilite a interação sendo objetivo(a), mas dando detalhes quando necessário.
- Durante a revisão, uma pessoa revisora pode abrir *threads* de solicitação de melhorias em seu código. Geralmente, é responsabilidade da pessoa revisora decidir quando a *thread* foi finalizada. 

.. important::
    O Querido Diário conta com vários repositórios e um número limitado de pessoas 
    mantenedoras para todos. Por isso, podemos demorar para revisar uma contribuição, 
    especialmente se ela não estiver relacionada a uma meta do projeto (mapeada no `roadmap`_).

    Caso tenha dúvidas sobre isso e gostaria de entender melhor como contribuir
    com revisão e/ou em tarefas prioritárias, entre em contato pelo Discord e participe
    da :ref:`reunião geral<Reunião Geral do Querido Diário>`. 


Mantendo
************

Responsabilidades de uma pessoa mantenedora do Querido Diário
================================================================

- Respeitar o :doc:`Código de Conduta<codigo-de-conduta>` e garantir que as pessoas tenham um ambiente seguro e acolhedor e que qualquer vítima de infração desse termo tenha um canal de ajuda
- Sempre justificar uma sugestão de acordo com as práticas já adotadas no projeto, legibilidade e simplicidade. É essencial que um projeto cívico tenha uma estrutura tão simples quanto possível para iniciantes
- O projeto deve ser testado antes de um *Pull Request* ser mesclado
- Manter o histórico de *commits* organizado, preferencialmente seguindo o formato a seguir onde toda alteração na base de código tem como base a *main* atualizada e é mesclada com um *merge commit*:

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/guide-commits-history.png
    :alt: Organização dos commits 

- Caso um Pull Request tenha muitos *commits* e as mensagens não forem claras, pode-se realizar um *squash* nos *commits* antes de mesclar o *Pull Request*

.. Referências
.. _git: https://pt.wikipedia.org/wiki/Git
.. _GitHub: https://docs.github.com/pt/get-started/quickstart/hello-world
.. _tutorial da Escola de Dados: https://escoladedados.org/tutoriais/introducao-ao-git-e-github-colaborando-com-projetos-de-codigo-aberto/
.. _Discussion do repositório querido-diario-comunidade: https://github.com/okfn-brasil/querido-diario-comunidade/discussions
.. _Discord da OKBR: https://go.ok.org.br/discord
.. _roadmap: https://github.com/orgs/okfn-brasil/projects/14/views/1
.. _repositório de comunidade: https://github.com/okfn-brasil/querido-diario-comunidade/issues
.. _agenda pública: https://go.ok.org.br/agenda-comunidade
.. _Quadros do Querido Diário: https://github.com/orgs/okfn-brasil/projects
