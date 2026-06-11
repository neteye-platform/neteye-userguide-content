Single Page Application in NetEye
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NetEye implements the possibility to work in **Single Page Application**
mode, in order to meet the requirements of all those users that need to
use a single NetEye module.

For all the users belonging to roles where the **Single Page
Application** is configured, NetEye will redirect them immediately to
the configured application right after the login. This means that the
user will not have access to the full NetEye sidebar and dashboard, but
will see only the applicationq he is allowed to use in fullscreen mode
(i.e. without topbar and sidebar).

How to correctly setup a role for Single Page Application
`````````````````````````````````````````````````````````

The **Single Page Application** is managed as an additional restriction.
To correctly set up a role for users that have access only to one of the
NetEye modules you should:

* set desired module permission as usual; for example if you want to
  grant user access to GLPI, you can follow the :ref:`GLPI configuration guide <glpi-permissions>`

* in the **Role Update** page that should be available for each particular role in (**Configuration** > **Access Control** > **Roles**),
  set the correct redirect URL in restrictions under the ``neteye-module``, where you will find
  the option ``/single-page-application/redirect-url``. Here, put the URL the user
  will be redirected to, upon login.

  .. note:: The setting is only valid granted it starts with /, otherwise it will be ignored.


.. note:: If a user belongs to more than one role with **Single Page
   Application** enabled, he will see the NetEye interface as usual,
   this means that he will not be redirected to one of the module, but
   he will land to NetEye main dashboard, and he will be able to
   navigate through NetEye using the sidebar.
