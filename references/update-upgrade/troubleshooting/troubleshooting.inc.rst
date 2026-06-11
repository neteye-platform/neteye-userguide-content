.. _update-ts:

Troubleshooting
===============

The Update and Upgrade procedures can stop for disparate reasons. This
section collects the most frequents cases and provide some guidelines
to resolve the issue and continue the procedures.

In some cases you might want to check out the logs of the various commands that have been executed.
All the logs are stored in a log file at :file:`/neteye/local/os/log/neteye_command/`

If you find a problem that is not covered in this page, please refer
to the official channels: sales, consultant or |support|_.
for help and directions on how to proceed.

Some check fails
----------------

In this case, an informative message will point out the check that
failed, allowing to inspect and fix the problem.

For example, if the exit message is similar to the following
one, you need to manually install the latest updates.

.. container:: codeblock

   .. code:: bash

      "Found updates not installed"
      "Example: icingacli, version 2.8.2_neteye1.82.1"

Then, after the updates are installed, you can run it again and the
command will start over the tasks.

.. _update-rpm-migration:

An :file:`.rpmnew` and/or :file:`.rpmsave` file is found
--------------------------------------------------------

This can happen in presence of a customisation in some of the
installed packages. Check section :ref:`rpm-migration` for directions
on how to proceed. Once done, remember to run :command:`neteye update`
again.


A cluster resource has not been created
---------------------------------------

During a NetEye Cluster upgrade, it can happen that there is the need
of creating new cluster resources before running the `neteye install`
script. Creation of a resource must be done manually, and directions can
be found in section :ref:`upgrade-additional-steps-cluster` of the :doc:`cluster-upgrade`.

An health check is failing
--------------------------

..  include:: /references/update-upgrade/troubleshooting/failing-health-checks.inc.rst

.. _check-cluster-status:

How to check the NetEye Cluster status
--------------------------------------

.. include:: /references/update-upgrade/troubleshooting/check-cluster-status.inc.rst

How to check DRBD status
------------------------

.. include:: /references/update-upgrade/troubleshooting/check-drbd-status.inc.rst

.. _es-upgrade-and-shard-relocation:

Elasticsearch Cluster upgrade
-----------------------------

.. include:: /references/update-upgrade/troubleshooting/es-shard-relocation-waiting.inc.rst
