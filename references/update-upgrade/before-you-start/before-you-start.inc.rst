

This section contains useful information and suggestions to guide you
in the process of successfully update and upgrade your NetEye
Single Node or Cluster installation.

The updates and upgrade procedures are almost completely automated
and require manual interaction only for breaking changes--improvements
that introduce changes that are not backward compatible with the
installed software, and for the cluster reactivation.

Recall that if your NetEye Nodes do not have direct Internet access,
the update and upgrade procedures require you to configure the proxy
configurations as explained in Section :ref:`nodes_behind_proxy`.

The **NetEye Update & Upgrade** section is organised in multiple
parts: First of all, this page contains a number of useful information
about **supporting tasks** that need to be carried out as part of the
whole Update & Upgrade procedure, especially **before** starting the
procedure: learn about :ref:`safe-command-execution`, how to
:ref:`migrate RPM configuration files <rpm-migration>`, read the
:ref:`release-notes`, and :ref:`obtain-changelog`

The **update process** for Single Node, Cluster, and Satellite
follows, see :ref:`update-procedure` and its subsections.

Then the heart of the **upgrade process** can be found in the
dedicated sections: :ref:`neteye-upgrade-single`,
:ref:`neteye-upgrade-cluster`, and :ref:`satellite-upgrade`.

Finally, a :ref:`update-ts` section helps you in tackling the problems
than may arise during the update and upgrade procedures.

.. _upgrade-overview:

Overview
--------

It is important to regularly update and upgrade your NetEye system in
order to reduce security risks and ensure the latest bugfixes are
applied.

If you want to upgrade NetEye and have past experience doing so, you
can skip directly to the section for upgrading from your current
version (the section titled “Upgrade from 4.\ **X** to 4.\ **Y**” in
the index at the left). You **MUST** upgrade sequentially from one
version to the next: **do NOT** skip versions, for instance going from
4.20 to 4.22.

Because NetEye adheres to the `Semantic Versioning Initiative
<http://semver.org/spec/v2.0.0.html>`_, the software we produce will
always fall into one of the following two categories:

* **Updates:** A set of minor changes, for instance bug fixes or
  clarifications to the documentation, which do not introduce new
  features or change expected behavior. Updates can be released
  weekly, or even daily, as the need arises.
* **Upgrades:** The introduction of new or significantly changed
  features, creating new interfaces, APIs and behaviors. Upgrades are
  planned in advance on a two-month release schedule, and are
  announced at the `NetEye Blog
  <https://www.neteye-blog.com/category/neteye/>`_.

.. _safe-command-execution:

Safe Command Execution
----------------------

.. include:: /references/update-upgrade/before-you-start/safe-command-execution.inc.rst

.. _rpm-migration:

Migrate .rpmsave and .rpmnew Files
----------------------------------

.. include:: /references/update-upgrade/before-you-start/rpm-migration.inc.rst

.. _untouchable-settings:

Protected Configuration Items
-----------------------------

.. include:: /references/update-upgrade/before-you-start/untouchable-settings.inc.rst

.. _release-notes:

Release Notes
-------------

You can find the full release notes on our blog via the following
link: `NetEye Release Notes
<https://www.neteye-blog.com/category/neteye/release-notes-2/>`_.

.. _obtain-changelog:

How to Obtain the Changelog
---------------------------

From version 4.6 onwards, the changelog can be created automatically,
on-demand. You can generate the changelog of the current release at any
time by following these steps.

First, install the changelog plugin for yum::

  # yum install yum-plugin-changelog -y

You can then view the changelog by passing the last release date to the
plugin, which will show all changes since that date. For instance, for
NetEye 4.8::

   # yum changelog 2019-07-31 all > changelog.txt
