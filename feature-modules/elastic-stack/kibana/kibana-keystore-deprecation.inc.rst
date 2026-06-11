Keystore Usage
~~~~~~~~~~~~~~

The Kibana Keystore feature comes with a *keybana-keystore* tool, which
permits to manage the settings in the keystore.

If your installation is a NetEye Cluster, you are advised to use
*kibana-keystore* tool **only from the cluster nodes where the Kibana
service is running**. If you have multiple kibana
instances running in the |ne| cluster, keep in mind that the *kibana-keystore* will
not be synchronized across the cluster nodes: any changes made
on one node will not be reflected on the other nodes.

Using the ``keybana-keystore`` tool from nodes where Kibana is not
running will have no effect on the Kibana Keystore configuration.
