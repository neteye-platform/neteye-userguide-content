Export and Import
-----------------

The Tornado GUI allows you to export parts, or all, of your Tornado Processing Tree in order to
reuse them or to backup the configuration.

Export Processing Node
++++++++++++++++++++++

Granted the Edit mode is enabled, you can export any subtree of the Tornado configuration in one of
the following ways:

1. Select :guilabel:`Export` in the dropdown menu on the right hand side of the node.
2. Use Download button in the top right corner of the node's detailed view.

The node will then be downloaded with all its children as a json filewith the file name
:file:`<hostname>-<node_name>-<timestamp>.json`

.. figure:: /core-modules/tornado/import-export/img/export_node.webp
   :alt: How to export a Tornado Processing Tree node


Import Processing Node
++++++++++++++++++++++

You can use the import feature to upload a previously downloaded configuration from Tornado.
When uploading you can import a node to replace any of the existing nodes in the Processing Tree
configuration, or choose a node for which you would like to import a child node.


Import Child Node
`````````````````

You can import the previously downloaded Processing Tree to any filter node as a child. To do that,
you can select :guilabel:`Import child node` from the :guilabel:`Add` dropdown menu on the top
right of the Tornado GUI window. Then you can select the parent node by clicking on it. After
selecting the file from your computer, press :guilabel:`Import` to upload the file. Should a node
with the same name already exist, then Tornado will refuse to create the node and you can select to
either overwrite the node or abort the import.

.. figure:: /core-modules/tornado/import-export/img/import_child_node.webp
   :alt: How to import an exported Tornado Processing Tree node as a child


Import Node to Replace
``````````````````````

If you want to overwrite a node in the Processing Tree with a downloaded configuration, then you
can select the option :guilabel:`Import to replace` in the dropdown menu of the node, or press the
import button on the top right of the node details.

.. figure:: /core-modules/tornado/import-export/img/import_replace_node.webp
   :alt: How to import an exported Tornado Processing Tree node to replace an existing node.

.. warning:: If you replace a node, all its children will also be overwritten!
