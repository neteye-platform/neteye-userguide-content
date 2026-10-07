
.. _alyvix_monitoring_integrations_neteye_checklist:

Quick Install Guide
```````````````````
To add Alyvix Service to a node in NetEye, follow these steps:

#. Install Alyvix Service according to its `installation instructions <https://alyvix.com/learn/service/install.html>`_
#. Configure :ref:`authentication <alyvix_nodes_authentication>` (certificates and JWT)
#. Choose a NetEye/Alyvix tenant architecture :ref:`(single or multi-tenant) <alyvix_nodes_architectures>`
#. Configure :ref:`multitenancy <alyvix_network_architecture>` and :ref:`role mappings <alyvix_role_mappings>` based on the chosen architecture
#. :ref:`Install the Alyvix Module <neteye-components>` in NetEye (if not already installed)
#. :ref:`alyvix_create_an_alyvix_node` as a Host in Director
#. Configure the Node (:ref:`license <alyvix_license_tab>`, sessions and test cases) :ref:`in NetEye's Alyvix module <alyvix_manage_node_details>`
#. Enable a test case, wait a few minutes, and then check that :ref:`reports <alyvix_test_case_reports_tab>` are available
#. :ref:`Configure the retention of metrics <alyvix_configure_metrics>` and check that :ref:`historical data in the ITOA module <alyvix_view_metrics>` is displayed
