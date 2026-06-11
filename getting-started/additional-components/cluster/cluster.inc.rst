.. _neteye-module-install-cluster-node:

Cluster Node
------------

The procedure to install a |ne| Component on a Cluster is slight more
complex, and requires some more effort than in a Single Node. The
steps are the following.

#. Like in the case of the Single Node, the first task to carry out is
   to :ref:`Update a NetEye Cluster <neteye-update-cluster>`.

#. Install the |ne| Component **on every node** of the cluster, using
   the same commands described in the :ref:`previous section
   <neteye-module-install-single-node>`, depending on the type of the
   |ne| Component to install.

#. Look for the template file having filepath with pattern
   ``/usr/share/neteye/cluster/templates/Services-{name}-*.conf.tpl``
   where ``{name}`` is the name of the |ne| Component you are
   installing, and the ``*`` is a wildcard for any string. If any such
   file does **not** exist, proceed to executing the neteye installation script
   once on any Cluster node (see step 5 below).


   If, on the contrary, any such file **exists**, adapt it to the
   settings of your cluster, and save it to a file with the same name
   without the ``.tpl`` suffix.

#. Now, for each file saved in the previous step, create the cluster
   resource by executing the following command on one of the nodes of
   the cluster

   .. hint:: Replace ``{name}`` with the name of the |ne| Component
      you are installing, and the ``*`` with the string that completes
      the actual filename.

   .. code:: bash

      cluster# /usr/share/neteye/scripts/cluster/cluster_service_setup.pl \
               -c /usr/share/neteye/cluster/templates/Services-{name}-*.conf

#. Execute the neteye installation script once on any cluster node:

   .. code:: bash

      cluster# neteye install

.. topic:: Example: Install the *Asset* Feature Module on a
   |ne| cluster.

   To install this Feature Module, start with the update of each node,
   then put all nodes in *standby*, except the |ne| Active Node.

   After performing :command:`dnf groupinstall
   neteye-asset --enablerepo=neteye` on each node of the cluster, on
   one node search the files with pattern
   :file:`/usr/share/neteye/cluster/templates/Services-asset-*.conf.tpl`,
   which are::

     /usr/share/neteye/cluster/templates/Services-asset-glpi.conf.tpl
     /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-ocsreports.conf.tpl
     /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-server.conf.tpl

   Then, adapt them to the cluster settings (in this case, modify the
   ``ip_pre``, ``cidr_netmask`` and check the ``drbd_minor`` and the
   ``drbd_port``) and save them in the files, respectively::

     /usr/share/neteye/cluster/templates/Services-asset-glpi.conf
     /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-ocsreports.conf
     /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-server.conf

   Finally, create the cluster resources with the commands


   .. code:: bash

      cluster# /usr/share/neteye/scripts/cluster/cluster_service_setup.pl -c /usr/share/neteye/cluster/templates/Services-asset-glpi.conf


   .. code:: bash

      cluster# /usr/share/neteye/scripts/cluster/cluster_service_setup.pl -c /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-ocsreports.conf

   .. code:: bash

      cluster# /usr/share/neteye/scripts/cluster/cluster_service_setup.pl -c /usr/share/neteye/cluster/templates/Services-asset-ocsinventory-server.conf

   At this point, installation is complete. Remember to run
   :command:`neteye install` once from any node and finally
   :ref:`refresh the module <access-new-module>`.
