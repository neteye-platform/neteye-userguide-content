.. _neteye-satellite-update:

Prerequisites
~~~~~~~~~~~~~

#. To update a Satellite it is required to have the configuration archive located in
   :file:`/root/satellite-setup/config/<neteye_release>/satellite-config.tar.gz`.


1. Run the Update
~~~~~~~~~~~~~~~~~

To automatically download the latest update you can run the following command on the **Satellite**:

.. code:: bash

   sat# neteye satellite update

After the command was executed, the output will inform if the update was successful or not:

* In case of successful upgrade you might need to restart NetEye to properly apply the updates.
  If the reboot is not needed, please skip the next step.
* In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.

2. Reboot
~~~~~~~~~

Restart NetEye to apply the updates correctly.

   .. code:: bash

      sat# neteye node reboot
