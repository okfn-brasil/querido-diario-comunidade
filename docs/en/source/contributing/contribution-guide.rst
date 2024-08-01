Contribution Guide
######################

This guide is intended to provide guidance to people on their journey with the *Querido
Diário* (Dear Diary) project. It outlines the initial steps that should be taken when
interacting with all of the project's repositories.

Each repository has specific characteristics, described in their own documentation.
Once you have finished reading these general guidelines, please read also the
`CONTRIBUTING.md` file of the repository you are interested in before making your contribution.

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

How to contribute
===========================

Knowing `git`_ and how the `GitHub`_ platform works is a crucial step. We recommend this
`Digital Ocean tutorial`_ if you are not familiar with them.

Issues
------------

On the **Issues** tab of the chosen repository, there are tasks or issues
specific to that repository. Check to see if any of them interest you.
- When a task is suitable for beginners, it will be labeled as `good first issue`. 
- When you find a task that looks interesting to you, comment that you will do that task. That way, other folks will know that you're already working on it, and can offer to help you or take on other tasks that haven't been worked on yet.
- Whether or not you've already chosen an *issue* to work on, you can also comment on relevant questions regarding the task.

Milestones
------------

If there are no open *issues* or none interest you, look at the **Milestones** of the
repository you want to contribute to. These indicate the goals to be achieved. Exploring
the project files looking for areas for improvement that would help the repository reach a
*milestone* is an excellent way of contributing.
- When you find something, open an *issue* describing the potential improvement. If you are also planning to work on that task, add that to the issue.

Community chat
------------------------

Join our `community`_. There, you'll be able to ask quick questions, interact with the project maintainers, learn about how can you
help and chat with other *Querido Diário* contributors.

Interacting with reviewers
====================================

- Be patient, and give reviewers some time. In addition to contribution reviews being complex and demanding time and attention by themselves, a reviewer can be - just like you - another contributor, who's volunteering their personal time when possible, or has other priorities before being able to review your contribution.
- Whether in the form of code or messages with questions and suggestions, try to be objective, but giving details when necessary, to facilitate interactions.
- During the reviewing process, a reviewer can open *threads* requesting improvements to your contribution. In general, it is the reviewers' role to decide when a *thread* has ended and mark it as "resolved" in the GitHub interface. That means that both of you agree with the implemented modifications and the contribution can move on.
- Sometimes the community organizes events for solving multiple issues in a short amount of time. Especially in these cases, an issue or pull request may be considered abandoned when is waiting for the contributor but doesn't receive updates for a couple of weeks. Just let us know you need more time, so we don't end up closing it by mistake.

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