Frontend
########

Na :doc:`../entendendo/arquitetura`, o frontend do projeto é a porta de entrada para a
maioria das pessoas conhecerem e usarem o projeto via sua interface web. Contudo,
foi desenvolvido em grandes fases (com lançamento em 2021 e modificações em 2022, com
a adição do `Querido Diário: Tecnologias na Educação`_) e ainda carece de documentação
e de mais especialistas que compreendam o projeto e saibam implementar e avaliar as
melhores práticas.

Para estruturar o desenvolvimento desta importante parte do projeto de forma
consistente, eficaz e colaborativa, precisamos que todas as pessoas contribuidoras
partam de uma base comum de definições sobre qual é a cara do QD e como ele se
apresenta para o mundo. Vamos iniciar esta conversa a seguir.

Design system
*************

O `design system`_ do Querido Diário está sendo desenvolvido seguindo a metodologia de
design atômico. Este documento fornece informações importantes para mantenedores do
projeto, tanto na área de frontend quanto de UI/UX design. Atualmente, estamos focando
na consolidação da base do design system.

Nesta fase inicial, estamos trabalhando em dois elementos fundamentais:

Guia de estilos
   Definição das diretrizes visuais básicas do projeto.
Mapeamento de componentes (átomos)
   Identificação e documentação dos componentes mais básicos da interface.

Após a consolidação da base, planejamos evoluir para organismos, páginas, etc. Este
processo nos permitirá trabalhar na interface do QD de maneira colaborativa de forma
mais compreensível por todas as pessoas envolvidas.

Recursos utilizados
===================

Para consolidar a base do design system, estamos utilizando:

* `Manual da marca do Querido Diário`_
* `Desenho de interfaces do MVP 1 do QD (2021)`_
* `Desenho de interfaces do MVP 2 do QD (2022)`_
* `Código existente do frontend`_

Estudos de referência
=====================

Os seguintes estudos foram realizados pela Juliana Holanda Bonomo, embaixadora de
Inovação Cívica da OKBR, e serão utilizados como base para nortear mudanças no projeto:

`Análise de usabilidade do QD`_
  Este estudo avalia a experiência de uso atual e identifica áreas de melhoria na 
  interface;
`Estudo de personas do QD`_
  Este estudo fornece insights sobre as pessoas usuárias típicas do Querido Diário, seus
  usos e objetivos.

Notas para pessoas mantenedoras
*******************************

* Ao desenvolver novos componentes, considere como eles se encaixam na hierarquia
  atômica (átomo, molécula, organismo, template ou página);
* Mantenha a documentação atualizada à medida que novos elementos são adicionados ao
  design system.

.. Referências
.. _Querido Diário\: Tecnologias na Educação: https://queridodiario.ok.org.br/educacao
.. _design system: https://www.figma.com/design/anOHB2av9QbYvDDhKCslds/Design-System-QD
.. _Manual da marca do Querido Diário: https://drive.google.com/file/d/14iQYyd8MroNu_9o2_OB8TceCQWfKKKQ5/view?usp=sharing
.. _Desenho de interfaces do MVP 1 do QD (2021): https://drive.google.com/file/d/1mrhc_WEAbtgBpKvrULwY1PTQD9EfNivI/view?usp=drive_link
.. _Desenho de interfaces do MVP 2 do QD (2022): https://drive.google.com/file/d/13NGtCz0t-nxwkYhtvP2I1GipH6C5Ee6F/view?usp=drive_link
.. _Código existente do frontend: https://github.com/okfn-brasil/querido-diario-frontend/blob/main/docs/CONTRIBUTING.md
.. _Análise de usabilidade do QD: https://drive.google.com/file/d/1YuR92YJwLyiOw2ucnqPxrzYy71BoglS_/view
.. _Estudo de personas do QD: https://drive.google.com/file/d/1nPQU3SkN9WuK5YxJTNQUEs-xWj-EZYrP/view
