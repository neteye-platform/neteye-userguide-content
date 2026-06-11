The :command:`neteye feature-module` command regards any enabled feature module in the |ne| system.
This command contains all the dedicated subcommands applicable to the installed feature modules.

``neteye feature-module add``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye feature-module add` lets you install a |ne| Feature Module on the system.
It performs the installation of the packages on all nodes of the cluster and
prepares the service files for the cluster resources related to the feature module.

To obtain the list of available feature modules, you can run the command:

.. code:: bash

   neteye# neteye feature-module add --help

The configuration files for the resources of the feature module can then be found in
:file:`/etc/neteye-services.d/<module_name>/` and can be edited as needed before proceeding with the installation.


``neteye feature-module neteye-elastic-stack``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye feature-module neteye-elastic-stack` command regards the NetEye Elastic Stack Feature Module
and is available only if the additional component is installed in the |ne| system.

.. _neteye-feature-module-neteye-elastic-stack-subscription-set:

``neteye feature-module neteye-elastic-stack subscription set``
```````````````````````````````````````````````````````````````
The :command:`neteye feature-module neteye-elastic-stack subscription set` command enables you to switch
between **Platinum** and **Enterprise** `Elastic Stack subscription levels <https://www.elastic.co/subscriptions>`__
based on your needs.
For example to activate the Enterprise subscription of Elastic Stack you can execute:

.. code::

    neteye feature-module neteye-elastic-stack \
    subscription set enterprise

.. note::

    Changing your subscription will affect the licensing plan of your installation.
    It is your responsibility to ensure you have the appropriate licensing in place before
    activating a new subscription. Please contact the `Customer Support <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/5>`__
    or the Sales office for more information.

The same command can be used for downgrading to a "Platinum" subscription.
Since certain functionalities might not be supported with the lower subscription level,
in this case it will be necessary to use the ``--force`` flag to acknowledge potential breaking
changes that could arise from downgrading the subscription.
