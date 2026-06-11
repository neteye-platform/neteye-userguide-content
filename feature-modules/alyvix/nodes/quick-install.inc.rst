
.. _monitoring_integrations_neteye_checklist:

Quick Install Guide
```````````````````
To add Alyvix Service to a node in NetEye, follow these steps:

#. Install Alyvix Service according to its `installation instructions <https://alyvix.com/learn/service/install.html>`_
#. Configure :ref:`authentication <alyvix-nodes-authentication>` (certificates and JWT)
#. Choose a NetEye/Alyvix tenant architecture :ref:`(single or multi-tenant) <alyvix-nodes-architectures>`
#. Configure :ref:`multitenancy <alyvix-network-architecture>` and :ref:`role mappings <role-mappings>` based on the chosen architecture
#. :ref:`Install the Alyvix Module <neteye-components>` in NetEye (if not already installed)
#. :ref:`alyvix-create-an-alyvix-node` as a Host in Director
#. Configure the Node (:ref:`license <alyvix-license-tab>`, sessions and test cases) :ref:`in NetEye's Alyvix module <manage-node-details>`
#. Enable a test case, wait a few minutes, and then check that :ref:`reports <alyvix-test-case-reports-tab>` are available
#. :ref:`Configure the retention of metrics <configure-metrics>` and check that :ref:`historical data in the ITOA module <view-metrics>` is displayed
