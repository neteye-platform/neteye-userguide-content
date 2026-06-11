How to Implement a Retention Policy
-----------------------------------

.. warning:: The Log Manager Retention Policy is now deprecated
   and is only applied to logs managed via rsyslog. The retention policy
   of logs managed via Real Time Log Signing must be configured with
   the `Elasticsearch Index Lifecycle Management
   <https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html>`__.

Suppose you would like to set a new company-wide retention policy for
all hosts in your system. The GDPR requires data minimization, and your
company has determined its policy should be 6 months maximum, but with a
2 year limit for a specific set of services run on a set of hosts
dedicated to multi-year customer subscriptions.

This How To will show you the steps necessary to set up this new
policy.  For further information about how Retention works, please see
the :ref:`Retention documentation <logmanager-retention-policy>`.

.. note:: Only rsyslog and Elastic Stack logs are governed by NetEye's
   retention policy mechanism.

Decide on a Retention Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Retention is determined host by host. But to avoid inefficiency when a
large number of hosts are involved, the retention policy is applied
hierarchically from most to least specific:

* A policy set on an individual host (most specific) will always prevail.
* If a host does not have a specific policy, then one of its host
  templates's policy will apply (with the standard hierarchical
  priorities of host templates)
* If there is not at least one policy on either the host or one of its
  templates, the default policy will apply

In our use case, the default should be 6 months (180 days), and for all
hosts set under one particular host template the retention policy should
be 2 years (730 days). We will thus need to change the system-wide
default from 365 to 180, and set the retention policy for that
particular host template to 730, leaving the retention policy field on
all hosts and all other host templates blank.

Implement the Retention Policy in the GUI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To implement the above policy, let's set the appropriate fields (these
can be done in any order):

#. To change the default retention policy from 365 days to 180,
   navigate to **Configuration > Modules > logmanager >
   Configuration** and change the value in the field "Default
   retention policy duration in days" to 180.

#. To change the policy on the host template, navigate to **Icinga
   Director > Host objects > Host Templates > [template] > Modify >
   Custom properties** and set the field "Retention policy days" to
   730 for the template you selected.

#. Deploy the changes from the previous step: **Icinga Director >
   Activity log > Deploy pending changes**. This is not necessary for
   the default policy, but if you change the retention policy on
   either a host template or a host, you will need to deploy before it
   will take effect since these are configured in Director.

You can now verify that the new retention policy has taken effect by
navigating to **Log Manager > Host** and checking that relevant hosts
have the correct number of days set in the :ref:`Retention Policy
<logmanager-deploy-hosts-safed-configurations>` column of the Log
Manager Host Configuration panel.

You can also check that the retention policy is being carried out
regularly. A specific service named **retention-policy-neteyelocal**
is :ref:`set up on the local monitoring machine
<neteye-check-creation>` to check that the retention policy is being
correctly applied. If it detects that logs are not being deleted, the
corresponding monitoring check on the dashboard will show red instead
of green.

.. rubric:: Optional: Enforce the Retention Policy in a Shell

Although the retention policy is :ref:`enforced automatically each day
after midnight <logmanager-retention-policy>`, you can manually do the
same thing when desired. The following shell commands use the Log
Manager API (root access required).

This will apply the retention policy to all hosts::

  # icingacli logmanager retentionpolicy apply

This will check that no log older than the retention period specified
for the corresponding host is still retained::

  # icingacli logmanager retentionpolicy verify
