Get to know Querido Diário
##########################

Objectives
**********

An official gazette is a publication made by the spheres of Brazilian public
administration, whether federal, state, or municipal, and from the executive,
legislative, and judicial branches. It works to **make official to the population
the decisions made by the powers**.

The Union Official Gazette and States Official Gazettes are often well-established
objects of collective interest, while Municipalities Official Gazettes, especially
from cities that are not part of a metropolitan region, are less watched. Not
surprising. Despite being public, **these documents are made available through
ways that are difficult to watch**.

Querido Diário is the project that tackles this data desert by offering **a tool
that expands access to information about Brazilian public administration** at its
most local level - the municipalities - through the opening and centralization of
electronic gazettes. It's not an easy endeavor, particularly because there are
**5570 municipalities** in the country and significant discrepancies regarding the
existence and maturity of the online availability of their data and information.

Pillars: the values of Querido Diário
*************************************

Understanding Querido Diário involves knowing where the project wants to go, its
values, and the resources it aims to offer.

1. Contextualized public information
====================================

We are committed to making gazettes available, always accompanied by which
municipality it concerns and its publication date. Cases where any of this data
cannot be obtained should not be integrated.

2. Complete availability
========================

The content of the gazettes, previously closed to PDFs, should be transformed
into plain text and stored in an open TXT format. Both files, the original and
the open one, are provided in their entirety by Querido Diário.

3. Huge searchable database
===========================

By gradually adding more municipalities from the 5,570 Brazilian municipalities,
we want to build a huge and pioneering database on local public administration,
enabling Brazilian society benefit from this information. To achieve this, we
will provide extensive search resources for one or multiple municipalities
simultaneously.

4. Open access
==============

Access to the data stored by Querido Diário and the search resources implemented
should be open and free for users in a friendly manner, and for developers or
machines, programmatically.

Facing the data desert
**********************

To realize its pillars, Querido Diário faces various situations related to the
characteristics of municipal official gazette publication in a context where there
is no national legislation to standardize it. In this section, we describe the
most relevant aspects that constitute obstacles being overcome by the project.

1. Electronic publication
=========================

For Querido Diário to have access to a municipality's gazettes, the municipality
must publish files on a website publicly available on the Internet. A
municipality that does not have an official website or does not make its gazettes
available online cannot be integrated into the project.

2. File format
==============

When we say "obtain official Gazettes," we are specifically interested in their
content: it is the text of the decree, the ordinance, the bidding, etc, that we
want; these are stored and shared through files.

Some file formats are more visual and suitable for human reading, while others
prioritize easy access to their textual content, being appropriate for machine
reading. Generally, municipalities prioritize human-readable publication and
provide their official acts through PDF files, which complicates the automatic
processing of their content. They come in two types:

   - **Text PDF**: the PDF content is text, likely generated directly by a text
     editor program.
   - **Image PDF**: the PDF content is an image, and the file was created from
     scanning the printed edition.

.. _type-gazettes:

3. Type of gazette
==================

Despite the lack of a legal standard, we find it possible to classify electronic
gazettes into three categories:

   - **Aggregated Gazette**: the document contains official acts from various
     municipalities, often because they are part of an Association/Federation or
     the State Gazette has a section for its municipalities. An example is the
     `Official Gazette of the Association of Municipalities of Alagoas`_.
   - **Complete Gazette**: all the official acts in the document correspond to a
     single municipality, such as the `Recife (PE) gazette`_.
   - **Fragmented Gazette**: there is no single document for all the official acts
     of the municipality; they are spread across multiple files. An example is
     `Bragança (PA)`_, which has sections for decrees, ordinances, etc., on its
     website, with various separated acts.

Note how these three types of gazzetes differ in terms of content granularity and
how they relate to each other as shown in the following diagram.

.. image:: https://querido-diario-static.nyc3.cdn.digitaloceanspaces.com/docs/qd-document-types.png
   :alt: Diagram about how gazette types relate. An aggregated gazette if splited each
         municipality makes a complete gazette and a fragmented gazette if all parts
         joined becomes a complete gazette.

.. REFERENCES:
.. _Official Gazette of the Association of Municipalities of Alagoas: https://www.diariomunicipal.com.br/ama/
.. _Recife (PE) gazette: https://dome.recife.pe.gov.br/dome/
.. _Bragança (PA): https://braganca.pa.gov.br/decretos-2023/
