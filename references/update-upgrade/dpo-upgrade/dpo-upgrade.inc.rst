.. _neteye-upgrade-dpo:

DPO machine Upgrade from |neteye_previous_version| to |neteye_version|
======================================================================

After performing a standard upgrade of your |ne| installation (see :ref:`neteye-upgrade-single` or
:ref:`neteye-upgrade-cluster`) you can upgrade the image of the containers running the verification of your
blockchains, previously configured with the :command:`neteye dpo setup` command, by running it again from
a |ne| Master node:

.. code:: bash

      neteye# neteye dpo setup

The command updates the container image at every execution, ensuring you are using the latest available
image matching your |ne| version, and restarts the already configured containers with the updated image.
