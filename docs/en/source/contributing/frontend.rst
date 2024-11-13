Frontend
########

In :doc:`../understanding/architecture`, the project's frontend is the main entry point for
most people to get to know and use the project via its web interface. However, it was
developed in large phases (launched in 2021 with modifications in 2022, with the addition of
`Querido Diário: Technologies in Education`_), and still needs documentation and more
specialists who understand the project and know how to implement and evaluate best practices.

To structure the development of this important part of the project in a consistent,
effective, and collaborative way, we need all contributors to start from a common base of
definitions about what QD looks like and how it presents itself to the world.
Let’s begin this conversation below.

Design system
*************

The Querido Diário `design system`_ is being developed following the atomic design methodology.
This document provides important information for project maintainers, both in frontend
and UI/UX design. Currently, we are focusing on consolidating the design system's foundation.

In this initial phase, we are working on two fundamental elements:

Style guide
   Definition of the project's basic visual guidelines.
Component mapping (atoms)
   Identification and documentation of the most basic interface components.

After the foundation is consolidated, we plan to evolve towards organisms, pages, etc.
This process will allow us to work on the QD interface collaboratively in a way that is
more understandable for all involved.

Resources used
==============

To consolidate the design system foundation, we are using:

* `Querido Diário brand manual`_
* `Interface design for QD MVP 1 (2021)`_
* `Interface design for QD MVP 2 (2022)`_
* `Existing frontend code`_

Reference studies
=================

The following studies were conducted by Juliana Holanda Bonomo, OKBR's Civic Innovation
ambassador, and will be used as a basis to guide changes in the project:

`QD usability analysis`_
  This study evaluates the current user experience and identifies areas for
  improvement in the interface;
`QD personas study`_
  This study provides insights into the typical users of Querido Diário, their
  uses, and goals.

Notes for maintainers
**********************

* When developing new components, consider how they fit into the atomic hierarchy (atom,
  molecule, organism, template, or page);
* Keep the documentation updated as new elements are added to the design system.

.. References
.. _Querido Diário\: Technologies in Education: https://queridodiario.ok.org.br/educacao
.. _design system: https://www.figma.com/design/anOHB2av9QbYvDDhKCslds/Design-System-QD
.. _Querido Diário brand manual: https://drive.google.com/file/d/14iQYyd8MroNu_9o2_OB8TceCQWfKKKQ5/view?usp=sharing
.. _Interface design for QD MVP 1 (2021): https://drive.google.com/file/d/1mrhc_WEAbtgBpKvrULwY1PTQD9EfNivI/view?usp=drive_link
.. _Interface design for QD MVP 2 (2022): https://drive.google.com/file/d/13NGtCz0t-nxwkYhtvP2I1GipH6C5Ee6F/view?usp=drive_link
.. _Existing frontend code: https://github.com/okfn-brasil/querido-diario-frontend/blob/main/docs/CONTRIBUTING-en-US.md
.. _QD usability analysis: https://drive.google.com/file/d/1YuR92YJwLyiOw2ucnqPxrzYy71BoglS_/view
.. _QD personas study: https://drive.google.com/file/d/1nPQU3SkN9WuK5YxJTNQUEs-xWj-EZYrP/view
