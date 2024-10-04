Contribution Guide
######################

This guide is intended to provide guidance to people on their journey with the *Querido
Diário* (Dear Diary) project. It outlines the initial steps that should be taken when
interacting with all of the project's repositories.

Each repository has specific characteristics, described in their own documentation.
Once you have finished reading these general guidelines, please read also the
`README.md` and `CONTRIBUTING.md` files of the repository you are interested in contributing, since they have more specific information about the development environment, installation, execution, goals, the dynamics of interaction in Pull Requests (PR), etc.

Code of Conduct
********************

To ensure our community is welcoming to all people, please read and follow our 
:doc:`Code of Conduct<code-of-conduct>` in all of our community spaces. **By participating 
in this project, you agree to be bound by the terms specified therein**.

Contributing
*****************

Ecosystem
=============

*Querido Diário* has several repositories, allowing one to contribute to
whatever they feel more affinity with:

- Scrapers - https://github.com/okfn-brasil/querido-diario
- Data Processing - https://github.com/okfn-brasil/querido-diario-data-processing
- API - https://github.com/okfn-brasil/querido-diario-api
- Backend - https://github.com/okfn-brasil/querido-diario-backend
- Frontend - https://github.com/okfn-brasil/querido-diario-frontend
- Documentation - https://github.com/okfn-brasil/querido-diario-comunidade

Finding out what to contribute
===========================

Knowing `git`_ and how the `GitHub`_ platform works is a crucial step. We recommend this
`Digital Ocean tutorial`_ if you are not familiar with them yet.

Issues
------------

On the **Issues** tab of the chosen repository, there are tasks or issues
specific to that repository. Check to see if any of them interest you.
- When a task is suitable for beginners, it will be labeled as `good first issue`. When it is an important task, aligned with the project goals, it will have the `priority` label. 
- When you find a task that looks interesting to you, comment that you will do that task. That way, other folks will know that you're already working on it, and can offer to help you or take on other tasks that haven't been worked on yet.
- If the task has a comment announcing someone's interest in working on it, but does not have a linked pull request and the comment is more than 1 month old, consider the issue available.
- Whether or not you've already chosen an *issue* to work on, you can also comment on relevant questions regarding the task.

Milestones
------------

If there are no open *issues* or none interest you, check the **Milestones** of the
repository. These indicate the goals to be achieved. Exploring
the project files looking for areas for improvement that would help the repository reach a
*milestone* is an excellent way of contributing.
- When you find something, open an *issue* describing the potential improvement and how it relates to one of the milestones. If you are also planning to work on that task, add that to the issue.

Boards
------------
Displayed in the Projects tab, **Boards** are resources that group issues of the same theme from one or more repositories in a centralized location, making it easier to navigate through available tasks. See what our `Boards`_ are.

Roadmap
------------
The `roadmap`_ is a special board that records the strategic direction that *Querido Diário* has on the horizon, the currently prioritized goals and the progress made.
- If you would like to suggest new goals for the roadmap, create an issue in the `community repository`_ using the "Roadmap Item" template.

Online community
------------------------
- Join the OKBR `Discord`_. There you can quickly ask questions, interact with the maintainers, ask how you can help, and even chat with other contributors.
- We also organize and promote activities on Discord such as sprints, regular meetings, pairings, on-call sessions, etc. All of these activities are open and can be followed through this `public calendar`_.

*Querido Diário* General Meeting
-------------------------------
Always on the last Thursday of the month, at 6 pm (São Paulo time/GMT-3), the *Querido Diário* General Meeting takes place on Discord, which is attended by the OKBR Civic Innovation team, maintainers and contributors.
- This meeting serves as an internal alignment of the community regarding the various development fronts that may be taking place and, on the other hand, it serves as a gateway for interested people, since it is a panoramic meeting, allowing them to get to know the people involved and the activities taking place.

Interacting with reviewers
====================================

- Be patient, and give reviewers some time. In addition to contribution reviews being complex and demanding time and attention by themselves, a reviewer can be - just like you - another contributor, who's volunteering their personal time when possible, or has other priorities before being able to review your contribution.
- Whether in the form of code or messages with questions and suggestions, try to be objective, but giving details when necessary, to facilitate interactions.
- During the reviewing process, a reviewer can open *threads* requesting improvements to your contribution. In general, it is the reviewers' role to decide when a *thread* has ended and mark it as "resolved" in the GitHub interface. That means that both of you agree with the implemented modifications and the contribution can move on.

Important
----------------------
*Querido Diário* has several repositories and a limited number of maintainers for each one. Therefore, it may take some time to review a contribution, especially if it is not related to a project goal (mapped in the `roadmap`_).

If you have any questions about this and would like to better understand how to contribute with review and/or priority tasks, please contact us via Discord and join the :ref:`general meeting<*Querido Diário* General Meeting>`.

Maintaining
************************

Responsibilities of a *Querido Diário* maintainer
================================================================

- Respect our :doc:`Code of Conduct<code-of-conduct>` and ensure that folks have a safe and welcoming environment, and that any victim of a breach of these terms has a support channel;
- Always justify a suggestion according to: the practices already adopted on the project, legibility and simplicity. It is essential that a civic project has as simple a structure as possible for newcomers;
- The project must be tested before a Pull Request is merged;
- Keep the commit history organized, preferably following the format below, where every repository change is based on the updated `main` and merged with a merge commit:

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/guide-commits-history.png
    :alt: Commits flow

- If a Pull Request has too many commits and its messages are not clear, it is possible to *squash* those commits before merging the Pull Request.


.. LINKS
.. _git: https://pt.wikipedia.org/wiki/Git
.. _GitHub: https://docs.github.com/pt/get-started/quickstart/hello-world
.. _Digital Ocean tutorial: https://www.digitalocean.com/community/tutorials/how-to-use-git-effectively
.. _community: https://go.ok.org.br/discord
.. _roadmap: https://github.com/orgs/okfn-brasil/projects/14/views/1
.. _Discord: https://go.ok.org.br/discord
.. _community repository: https://github.com/okfn-brasil/querido-diario-comunidade/issues
.. _Boards: https://github.com/orgs/okfn-brasil/projects?query=is%3Aopen
.. _public calendar: https://go.ok.org.br/agenda-comunidade
