:orphan:

.. _ug_guidelines:

===============================
 User Guide Content Guidelines
===============================

This section contains directions to assist in both writing new content
and reviewing existing content for |ne|\ 's user guide.

This section is organized as follows. We first introduce some
:ref:`ug-terminology`, then give examples of :ref:`ug-roles` and
:ref:`ug-directives`, and finally show how to write :ref:`ug-prompts`
in code blocks.


.. _ug-style:

Language Style
==============

This section lists conventions we are using and that need to be
followed when writing new content or updating existing content.


.. csv-table::
   :widths: 10,20,70
   :header: "Rule", "Name", "Description"

   "**L1**", "Active tone", "Use active voice instead of passive,
   unless the latter is required by the context. Compare (passive):
   *To begin monitoring an Alyvix node, the node must first be created
   in the Director module as an Icinga Host* with (active): *To begin
   monitoring an Alyvix node, you should first create one in the
   Director module as an Icinga Host*"
   "**L2**", "Direct tone", "Use direct, personal tone when giving
   specific instructions to users, e.g.: *Go to the Downloads section
   of the NetEye blog, then move to NetEye â€“-> GPG public key and save
   public-gpg-key.*"
   "**L3**", "Section titles", "Use nouns or noun groups instead of
   imperative sentences in section titles of all levels, e.g. *Manual
   blockchain verification* instead of *Manually verify blockchain*"
   "**L4**", "Titles capitalization", "Use Capital Letters in section
   titles of all levels, with the exception of auxiliary words,
   e.g. articles, prepositions, and the like"
   "**L5**", "Commas", "Do not use a comma after i.e. and e.g."

.. _ug-terminology:

Terminology
===========

Consistent use of terminology and style of words is mandatory in a
guide.

Namimg
------

.. csv-table::
   :widths: 10,90
   :header: "Rule", "Description"

   "**N1**", "Capitalise all names that relate to |ne|, including its
   modules and all the terms that refer to |ne| elements. Each term
   should have an entry in the glossary, so it can be referenced by
   using the ``:term:`` role."
   "**N2**", "NetEye specific terms do not require articles. This
   refers to both original NetEye terminology, e.g. |nei|, *Single
   Node*, and terms related to software integrated with NetEye,
   e.g. *Telegraf*, *Icinga 2*, not *the Telegraf*, *the Icinga 2*"

|ne| Architecture
~~~~~~~~~~~~~~~~~

All terms related to |ne| are written with an uppercase first
letter, except for |ne| itself, for example:

.. csv-table::
   :widths: 10,90
   :header: "Rule", "Description"

   "**NE1**", "|neb| is written in `Pascal Case / upper camel case
   <https://en.wikipedia.org/wiki/Camel_case>`_. Also `substitutions
   <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#substitutions>`_
   have been added to the configuration: ``|ne|`` will always expand
   to |ne|; ``|neb|`` to |neb|, and ``|nei|`` to |nei|"
   "**NE2**", "`(NetEye) Single Node`, `(NetEye) Cluster Node`,
   `(NetEye) Satellite Node`, `(NetEye) Master-Satellite`"
   "**NE3**", "`Elastic-only`, `Voting-only`, but note that sometimes
   they are shortened as **(E)** and **(V)**, respectively"
   "**NE4**", "`Single Purpose nodes` (used to denote both
   `Elastic-only` and `Voting-only` nodes)"

.. note:: In rule **NE2**, the |ne| prefix should always be used,
   because it is part of the name, but it can be omitted on a
   case-by-case basis only if it becomes redundant, for example in
   paragraphs in which the term is used multiple times.

As additional remark, do not capitalise terms like cluster, cluster
node, etc, if you are not referring to |ne| elements, for example:
*RHEL 8 cluster*. Pay also attention to capitalise properly:
`Elastic-only` is correct, `Elastic-Only` is not.

|ne| Modules and Related Terminology
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The names of |ne| modules, of software components used in them,
and specific terminology should always be capitalized.

.. csv-table::
   :widths: 10,90
   :header: "Rule", "Description"

   "**NM1**", "`NetEye Components` denotes the union of `NetEye
   Feature Modules` (or `NetEye Modules`), `Preview Software`, and
   `Beta Software`"
   "**NM2**", "Shutdown Manager, Log Management, El Proxy, Retention
   Policy"
   "**NM3**", "Make sure to respect upstream naming convention,
   including `Icinga 2`, `Icinga Web 2`, `NATS` (`NATS server`, `NATS
   client`), `NGINX`, `InfluxDB`, `RHEL 8`, `ntopng`, `ClickHouse`
   etc."


.. rubric:: Additional miscellaneous terms

* multi-tenancy

* downtime (uncountable)

* RHEL 8 (**not** RedHat 8, RHEL8 or other variations)

* CRITICAL, WARNING, DOWN, etc (always in all-capital when referred
  to the status of a |ne| health check)

Various Expressions
~~~~~~~~~~~~~~~~~~~

* to tell readers who they should contact for help/information, copy
  and paste (you might need to change the introductory `please refer
  to`) the following source code::

    please refer to the official channels: sales, consultants,
    or |support|_.

  It will render as: please refer to the official channels: sales,
  consultants, or |support|_.

  .. hint:: The replacement ``|support|_`` will always expand to |support|_
     (with hyperlink).

Configuration Options, GUI Elements, Files/Paths
------------------------------------------------

.. csv-table::
   :widths: 10,90
   :header: "Rule", "Description"

   "**C1**", "The name of configuration options and of elements in the
   GUI is always in **boldface**: this includes labels, tab names
   (e.g. **Host**, **Services**, **History**), but not buttons."
   "**C2**", "To denote a button or an interactive element of a GUI,
   use the ``:guilabel:`` role, see :ref:`ug-roles`."
   "**C3**", "Values of configuration options, including default and
   suggested values, are written in `italics`, e.g. 'The **Retention
   Policy** defaults to `15 days`. To avoid resource exhaustion in
   case of high traffic, you can reduce it to `10` or `7` days.'"
   "**C4**", "When mentioning files or (standard) filesystem locations
   always use the **full path** and the ``:file:`` role,
   e.g. ``:file:`/neteye/shared/tornado/```. Do not use placeholders
   like ``{grafana_conf_dir}``, ``{data_backup_dir}``,
   ``$config-dir``, or similar."
   "**C5**","*Technical* terms, like for example the name of databases
   and of their tables, should be written in monospace font using the
   double backticks, like e.g. * See the ``icinga2_restarts`` table of
   the ``director`` database.*"

.. _ug-roles:

Roles
=====

Roles are used inline with the content to identify and highlight
certain portions of text.

``:term:``
   It creates a cross-reference to a term defined in the glossary. The
   term must exist in the glossary. Example: \:term:\`Downtime\` becomes
   :term:`Downtime`.

``:abbr:``
   It is used to create tootips when hovering. Example: \:abbr:\`SLM
   (Service Level Management)\` becomes :abbr:`SLM
   (Service Level Management)`

``:file:``
   A role used to show the location of a file on the
   filesystem. Example:
   \:file:\`/neteye/shared/icingaweb2/conf/authentication.ini\` becomes
   :file:`/neteye/shared/icingaweb2/conf/authentication.ini`

``:command:``
   Similar to previous, it shows a command. Example:
   \:command:\`neteye status\` becomes :command:`neteye status`

``:guilabel:``
   This role is used for (G)UI's interactive parts (mostly
   buttons). Example: \:guilabel:\`Add a new user\` becomes
   :guilabel:`Add a new user`.

``:menuselection:``
   This helps when a user needs to be guided through (G)UI's
   menus. Example: \:menuselection:\`Configuration / Authentication
   / Users\` becomes  :menuselection:`Configuration / Authentication
   / Users`

``:phone:``
    Phone class has to have white-space: nowarp.
    :phone:`+39 0471 56 41 01`

``:ref:``
    Used to create a reference to a label (which can be in every file,
    not just the current one) and create a clickable link, for
    example: \:ref:\`ug-terminology\` becomes
    :ref:`ug-terminology`.

    It can be used also with a custom text. For example: Please check
    the \:ref:\`default terminology <ug-terminology>\` becomes Please
    check the :ref:`default terminology <ug-terminology>`

``:numref:``
   Create a numbered reference to a figure or code listing. It works
   as ``:ref:`` but outputs the number of the
   figure. \:numref:\`figure-neteye-cluster\` becomes
   :numref:`figure-neteye-cluster`.  When using ``:ref:`` with a
   label, instead of the number, the *caption* of the figure is used:
   \:ref:\`figure-neteye-cluster\` becomes
   :ref:`figure-neteye-cluster`.

.. _ug-directives:

Directives
==========

Directives are used to separate content from surrounding text and are
used to emphasise content or to provide additional information to the
surrounding text.

.. rubric:: This is a ``rubric``

Rubric is used to create a non-sectioning heading, i.e. a heading that
does not appear in the ToC. Rubrics are used in this section
(see above). Example::

  .. rubric:: This is a ``rubric``

.. rubric:: seealso

Useful directive to add bibliography, (external) reference articles,
or other content in the UG. Example::

  .. seealso:: `Grafana customisation
     <https://www.neteye-blog.com/2021/03/how-to-customize-a-grafana-component/>`_
     is explained in the `NetEye Blog <https://neteye-blog.com/>`_.


.. seealso:: `Grafana customisation
   <https://www.neteye-blog.com/2021/03/how-to-customize-a-grafana-component/>`_
   is explained in the `NetEye Blog <https://neteye-blog.com/>`_.

.. rubric:: Admonitions

Admonitions are used to draw attention on something just written. Examples::

  .. note:: Remember to save the setting.

  .. hint:: The default value is **5**.

  .. warning:: Make a backup copy before upgrading!

  .. tip:: Check also Figure 3.

.. note:: Remember to save the setting.

.. hint:: The default value is **5**.

.. warning:: Make a backup copy before upgrading!

.. tip:: Check also Figure 3.


.. rubric:: topic

A topic is used to extend and complement the knowledge of the
surrounding text, but reading it is not necessary to the actual
content of the page/text. Example::

  .. topic:: When to use roles and directives in Sphinx

     Roles and directives provide a great means to improve text layout
     and organisation, allowing the reader to find quickly the
     important pieces of the documentation.

.. topic:: When to use roles and directives in Sphinx

   Roles and directives provide a great means to improve text layout
   and organisation, allowing the reader to find quickly the important
   pieces of the documentation.

Importing Additional Sections
=============================

In order to maintain order when creating a large page with a considerable number of sections and subsections,
it is possible to divide the page content into smaller files, to be imported in the page.

In order to accomplish this goal, the next guidelines can be followed:

*  Create a folder at the same level with the ``.rst`` file of the page, with the name of the page:

   **Es.**: Having the page in the file :file:`page.rst`, the folder will be named :file:`page` and it will
   contain all the subsections included in the page.
*  Name each section, included as external files in the page with the ``.inc.rst`` format. All the included files
   should be created inside the :file:`page` folder.
*  Import all the desired files in the :file:`page.rst` using the ``.. include::`` command:

   .. code::

        .. include:: page/section.inc.rst


.. _ug-prompts:

Command Line Prompts
====================

The following prompts must be used whenever writing a ``code-block::``.

.. csv-table:: Command prompts
   :header: "Type", "Description", "Command Example"
   :widths: 30, 30, 40
   :file: command-prompts.csv
