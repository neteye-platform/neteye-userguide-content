
Some templates may require indices to have at least one
`replica <https://www.elastic.co/guide/en/elasticsearch/reference/current/scalability.html>`__,
which in simple terms means that the index must be replicated on more than one node.
While having replicas is straightforward on a NetEye Cluster, on Single Node installations
this is not possible, which causes these indices
to go in *yellow* state due to the so-called **unassigned shards** warning.

Since on Single Node installations this warning cannot be solved by assigning
shards to other nodes, the solution to the problem is to tell Elasticsearch
that non-replicated indices should be allowed.

NetEye applies this solution by default during the :term:`neteye install` on the
templates and indices which have this problem. However, some templates and indices are
created only when some features of the Elastic Stack are triggered by the user and
must be manually fixed by the Elastic Stack administrator.

To facilitate the job of admin though, NetEye provides a set of scripts which
help to fix the problem by setting the option
`index.auto_expand_replicas <https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#dynamic-index-settings>`__
to **0-1** on the templates allowing the indices to have **zero**
replicas on Single Node installations.

Depending whether the problematic index is managed by Fleet or not, you may use
the appropriate script.

Index Templates Managed by Fleet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To fix index templates managed by Fleet (for example those that are automatically created when installing
a new `Elastic integration <https://www.elastic.co/integrations/data-integrations>`__) you can use the following script that
will resolve this issue on **all** the Fleet-managed index templates and already created associated indexes.

.. code:: bash

   neteye# python3 /usr/share/neteye/elasticsearch/scripts/configurator/fix_fleet_integrations_autoexpand_replicas.py


Index Templates Not Managed by Fleet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Suppose that, after the creation and execution of a Security rule, you identified that the Elasticsearch template
that causes the index to have the number of replicas set to one (or more) is named
**.items-default**. You can then call the script as follows:

.. code:: bash

   neteye# bash /usr/share/neteye/elasticsearch/scripts/elasticsearch_set_autoexpand_replicas_to_index_templates_and_indexes.sh ".items-default"

.. note::
   The script works only with the composable index templates introduced in
   Elasticsearch 7.8 and does not support the *Legacy index templates*.

   Moreover, the script supports the update of multiple Index Templates at once.
   To perform such operation simply pass the multiple Index Templates names as
   arguments, like this:

   .. code:: bash

      neteye# bash /usr/share/neteye/elasticsearch/scripts/elasticsearch_set_autoexpand_replicas_to_index_templates_and_indexes.sh <index_template_name_1> <index_template_name_2> <index_template_name_3>
