``neteye_finalize_installation`` is the last command executed during
an upgrade procedure and makes sure that the correct NetEye version is
stored. It is the last task of :command:`neteye upgrade`.

.. note:: This command should never be used in the update and upgrade
   procedures, as it is called automatically by the :command:`neteye
   update` and :command:`neteye upgrade` commands. In case you need to
   launch it manually, follow the steps described below.

Complete the upgrade process by launching the following script::

   # neteye_finalize_installation

.. note:: You should launch the *finalize* command only if you want
   to perform the upgrade **manually** and only if all
   previous steps have been completed **successfully**. If you
   encounter any errors or problems during the upgrade process, please
   contact our `our service and support team <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portals>`__ to
   evaluate the best way forward for upgrading your NetEye system.
