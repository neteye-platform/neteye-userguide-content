
.. _install-alyvix-node:

Install an Alyvix Node
```````````````````````
In order to start using Alyvix Service, first you have to install
an Alyvix Node as recommended in the official `Alyvix Service user guide <https://alyvix.com/learn/service/install.html#installation-steps>`_.

.. note::

    NetEye supports Alyvix Service version >= 2.5.0. Please make sure your Alyvix Node is up to date before
    proceeding with the installation.

As a part of the installation procedure, you will have to perform mandatory security
configuration. Appropriate certificate, JWT and role mapping files you will need
are provided in :ref:`alyvix-nodes-authentication`.

After the installation has been completed, you can proceed to configuring the Alyvix Node
in the Director Module.


.. _alyvix-network-architecture:

Alyvix Network Architecture
+++++++++++++++++++++++++++

Before you start configuring the Alyvix Node in NetEye,
it is important that you decide on the type of your network architecture
and the type of environment you would like to work in, i.e. single- or multi-tenant.

There are four types of Alyvix Nodes for you to choose from when configuring an Alyvix Node
intended to run the service. Thus, it is important that you are familiar with
the :ref:`alyvix-nodes-architectures` at the moment of carrying out :ref:`install-alyvix-node` step,
so that you can already define which type of a :ref:`role-mappings` is to be later applied
at step :ref:`alyvix-create-an-alyvix-node`.

.. note:: In case you prefer to set up your infrastructure as a multi-tenant environment,
   you need to create one or multiple Tenants in addition to the Master Tenant,
   which serves as a default one. In order to do that,
   use the :command:`neteye tenant config create`. For more information please
   see :ref:`neteye-command` section.

For |ne| to be able to reach the Alyvix Service, port 443 is to be opened,
while the Alyvix should reach NetEye through port 4222.
For a deeper insight into the Network Architecture please refer to the diagrams
in :ref:`alyvix-nodes-architectures` section.

.. _alyvix-neteye-ver-compatibility:

Version Compatibility with Neteye
+++++++++++++++++++++++++++++++++

In order to have the integration working, NetEye and Alyvix service should support
the same API version. New features of Alyvix, present with new API versions, can only
be used in NetEye granted the latter supports the new API version.

The upgrade of NetEye and Alyvix is done individually.

`Alyvix service version <https://alyvix.com/learn/service/install.html#versions>`__
serves as an indicator of which API version is exposed by each Alyvix service version.


NetEye |neteye_version| version supports Alyvix API v3, v4, v5.


.. _alyvix-nodes-authentication:

Alyvix Node Authentication
``````````````````````````

|ne| and the Alyvix nodes need to communicate securely, thus
HTTPS certificates must be installed and JWT authentication must be configured
for each Alyvix node.



Certificates
++++++++++++

Each Alyvix node needs to have HTTPS certificates installed to enable
secure communication with |ne|.

We **strongly recommend** following the same procedure as for the installation of
the certificates on |ne| itself. The :ref:`neteye-https-conf` section
explains the procedure in details.

Another, **not recommended**, way to install the certificates on the Alyvix nodes
is to create the certificates via a script provided by |ne|.
The script generates server certificates signed by the internal Root CA of NetEye,
which will then be trusted only by your NetEye machines.

.. warning::
   Installing the certificates via the |ne| script is not recommended
   and it is only intended as a temporary solution.

The script is located under ``/usr/share/neteye/scripts/security/generate_server_certs.sh``
and it accepts the following positional parameters:

#. Distinguished name. Example `/C=IT/ST=Bolzano/L=Bolzano/O=Global Security/OU=Neteye/CN=*.neteyelocal`
#. Server DNS(s), separated by whitespaces, for which the certificate
   must be valid
#. IP(s), separated by whitespaces, for which the certificate must
   be valid
#. Filename with which the certificate will be saved on file system
#. [Optional] Output folder, which defines the location of the certificates (defaults to ``./``)

Example usage:

.. code:: bash

   bash /usr/share/neteye/scripts/security/generate_server_certs.sh \
   "/C=IT/ST=Bolzano/L=Bolzano/O=GlobalSecurity/OU=AlyvixNodes/CN=example03.lan" \
   "example03.lan" 192.168.0.100 "example03" "/root/certs"

The generated certificates must be copied and renamed inside the Alyvix nodes according to the documentation.
For the details on where to store the certificates on the Alyvix nodes, please consult the
`official Alyvix documentation <https://alyvix.com/learn/service/install.html#installation-steps>`_.

JWT Authentication
++++++++++++++++++

When performing a request from |ne| to the Alyvix node,
Alyvix needs a way to authenticate the request.

`JSON Web Tokens <https://www.jwt.io>`__ (JWT) are used to authenticate the
requests to the Alyvix nodes.
JWT is an open industry standard defined in :rfc:`7519` that provides a
compact and self-contained way for securely transmitting information
between parties as a JSON object.

Every request to Alyvix will contain the JWT Authorization Header with the JWT
token retrieved from |ne|. The JWT is calculated using the ``RS512`` algorithm
(see :rfc:`7518#section-3.3`) and the
key pair, which is stored on the |ne| host under
:file:`/neteye/shared/icingaweb2/conf/modules/neteye/jwt-keys`.

In order for Alyvix to verify the JWT token, copy the public key file
:file:`/neteye/shared/icingaweb2/conf/modules/neteye/jwt-keys/multipurpose-jwt.pub`
inside each Alyvix node. For more information please refer to the
`official Alyvix documentation <https://alyvix.com/learn/service/install.html#installation-steps>`_.

.. _role-mappings:

Role Mappings
+++++++++++++

For Alyvix to correctly evaluate the permissions defined in the JWT and thus correctly
authenticate the user of the JWT, the following mappings need to be copied inside each Alyvix node.
For more information, please refer to the
`official Alyvix documentation <https://alyvix.com/learn/service/monitoring_integrations/neteye_integration.html#monitoring-integrations-neteye>`__.
Please select the proper mapping based on the architecture of the Alyvix node to be configured.

**Role mappings for** :ref:`alyvix-multitenant-tenant-shared-node`

.. code:: json

   {
     "role": {
         "admin" : [
           [
             "$.permissions[?(@ == '*')]"
           ]
         ],
         "editor": [
           [
             "$.permissions[?(@ == 'module/alyvix/*')]"
           ],
           [
             "$.permissions[?(@ == 'module/alyvix/edit')]"
           ]
         ],
         "viewer": [
           [
             "$.permissions[?(@ == 'module/alyvix')]"
           ]
         ]
     },
     "tenants": "$.restrictions.neteye_tenants"
   }

**Role mappings for** :ref:`alyvix-multitenant-tenant-specific-node`

.. note:: Please replace every occurrence of ``<tenant_name>`` in the mapping below
   with the actual Tenant name before copying the mapping into the Alyvix nodes.

.. code:: json

   {
     "role": {
         "admin": [
           [
             "$.permissions[?(@ == '*')]"
           ],
           [
             "$.permissions[?(@ == 'module/alyvix/*')]",
             "$.restrictions.neteye_tenants[?(@ == '<tenant_name>')]"
           ]
         ],
         "editor": [
           [
             "$.permissions[?(@ == 'module/alyvix/edit')]",
             "$.restrictions.neteye_tenants[?(@ == '<tenant_name>')]"
           ]
         ],
         "viewer": [
           [
            "$.permissions[?(@ == 'module/alyvix')]",
            "$.restrictions.neteye_tenants[?(@ == '<tenant_name>')]"
           ]
         ]
     },
     "tenants": "$.restrictions.neteye_tenants"
   }

**Role mappings for** :ref:`alyvix-single-tenant-direct-to-master-node` **and for** :ref:`alyvix-single-tenant-via-satellite-node`

.. code:: json

   {
     "role": {
         "admin": [
           [
             "$.permissions[?(@ == '*')]"
           ],
           [
             "$.permissions[?(@ == 'module/alyvix/*')]",
             "$.restrictions.neteye_tenants[?(@ == 'master')]"
           ]
         ],
         "editor": [
           [
             "$.permissions[?(@ == 'module/alyvix/edit')]",
             "$.restrictions.neteye_tenants[?(@ == 'master')]"
           ]
         ],
         "viewer": [
           [
            "$.permissions[?(@ == 'module/alyvix')]",
            "$.restrictions.neteye_tenants[?(@ == 'master')]"
           ]
         ]
     },
     "tenants": "$.restrictions.neteye_tenants"
   }



.. _alyvix-create-an-alyvix-node:

Configure an Alyvix Node
````````````````````````

To begin monitoring an Alyvix node, the node must first be
created in the Director Module as an Icinga Host, setting
the **Hostname** and the **Host address**,
see :numref:`figure-alyvix-host-creation`.

Moreover, select the preferred option in the `Alyvix node` field under
the :menuselection:`Alyvix settings` section.
For more information regarding this setting, please refer to
:ref:`alyvix-nodes-architectures`.

.. _figure-alyvix-host-creation:

.. figure:: /feature-modules/alyvix/img/alyvix-host-creation.png
   :alt: Creation of a Host as an Alyvix node

   Creation of a Host as an Alyvix node

Once all the settings have been configured, a deployment is needed.
A newly created Node is to appear in the Nodes list, where you can manage
its details, and in the :ref:`dashboard`.

.. _alyvix-nodes-list:

Visualize Nodes
```````````````

The Alyvix node list can be visualized by visiting the :menuselection:`Alyvix / Nodes` page.
The following fields are displayed for each node:

.. |disconnect_icon| image:: /feature-modules/alyvix/img/alyvix-disconnected-icon.png
   :height: 24px
   :width: 24px

.. |running_icon| image:: /feature-modules/alyvix/img/alyvix-running-icon.png
   :height: 24px
   :width: 24px

.. |waiting_icon| image:: /feature-modules/alyvix/img/alyvix-waiting-icon.png
   :height: 24px
   :width: 24px

.. |stopped_icon| image:: /feature-modules/alyvix/img/alyvix-stopped-icon.png
   :height: 24px
   :width: 24px

* **Name**: The name of the Alyvix node set previously during the Host creation
* **Tenant**: The Tenant which the node belongs to. In case of a multitenant tenant-shared node, an icon
    is displayed instead of the tenant names.
* **Sessions status**: The list of sessions with the status. Possible status values are:

   * |disconnect_icon| *Disconnected* (Alyvix service is not able to connect to a specific session)
   * |running_icon| *Running* (Alyvix service is connected to the session and the session is running)
   * |waiting_icon| *Waiting* (Alyvix service is connected to the session and is ready to run assigned test cases)
   * |stopped_icon| *Stopped* (Alyvix service is connected to the session and is not going to run test cases)

* **Health**: The Host status monitored by NetEye. Next to the health status is a link that opens the host page in the :ref:`monitoring module <active-monitoring>`.
* **License**: The status of the license can be: *Disabled*, *Active*, *Expiring* or *Expired*. See more on this in the :ref:`alyvix-license-tab`.
* **Alyvix version**: The version of Alyvix that is running on the node

Apart from the node details, a summary provides the following information:

* **Total nodes**: The overall number of nodes
* **License alerts**: The number of licenses that are not in the *Active* status
* **Health alerts**: The number of nodes with the host status *DOWN* or *UNREACHABLE*

For an easier lookup of the relevant information related to the nodes and their execution,
it is possible to search the table, by using the search bar, and sort the various columns,
clicking their name.

.. _figure-alyvix-nodes-page:

.. figure:: /feature-modules/alyvix/img/alyvix-nodes-page.png
   :alt: The Alyvix nodes page

   The Alyvix nodes page


.. _manage-node-details:

Manage Node Details
```````````````````

From the nodes page, available at :menuselection:`Alyvix / Nodes`, it is possible to click a particular
node in the list to visualize its details. The details panel,
shown in :numref:`figure-alyvix-nodes-session-list`, is then rendered
on the right-hand side of the table, displaying the details of the node, grouped in three different tabs.

.. _alyvix-sessions-tab:

Sessions Tab
++++++++++++++

A session on an Alyvix node is defined by the following properties:

* **Name**: The domain and the session username separated by a ``\`` . E.g. ``WP\MyUsername``
* **Testcase waiting period**: Number of minutes before scheduling the next test case
* **Workflow waiting period**: Number of minutes before restarting the workflow
* **Display dimensions**

   * **Width**: the width of the screen resolution in pixels
   * **Height**: the height of the screen resolution in pixels
   * **Zoom**: the zoom percentage

* **Tenant**: the Tenant to which the session refers. This setting is only applicable to
  :ref:`alyvix-multitenant-tenant-shared-node` nodes, and it is only used for the collection
  of the performance metrics. Furthermore, this setting can be edited only by |ne| users with
  the :ref:`alyvix-super-admin-role` role. For more information regarding the roles and permissions
  on the Alyvix module, please refer to the :ref:`User Roles <alyvix-permissions-roles>`
  section.

  .. note:: Currently no validation is applied to the Tenant name value when it's being modified
     and in order for the changes to take effect, the :ref:`neteye alyvix-node setup <neteye-alyvix-node-setup>`
     command should be used.

* **Password**: the password used by the session in conjunction with the username

The `Sessions` tab, shown in :numref:`figure-alyvix-nodes-session-list`, contains:

* The `New session` button, which allows a user to create a new session
* A **counter**, that reports how many licensed sessions have been consumed out of the available ones
* The **sessions table**, in which each row represents a separate session defined on the node.
  Each session on the node can be edited or deleted by selecting the preferable action in the
  `More options` menu for a particular session.
  When editing a session a user can:

  * Modify the sessions properties in the `Settings` tab. Please note that the **Name** cannot be changed
  * Manage the session workflow in the `Workflow` tab

  A change in the session workflow status can also be applied to multiple sessions,
  by selecting the corresponding rows in the tab and applying the desired action.

.. _figure-alyvix-nodes-session-list:

.. figure:: /feature-modules/alyvix/img/alyvix-session-list.png
   :alt: The session list of a node

   The session list of a node

.. _control-session-workflow:

.. topic:: Control session workflow

   The workflow of a session is a set of active test cases that will be
   executed by a session on the Alyvix node.
   The list of test cases of a session is available in the `Workflow` tab
   displayed via the `Edit` item of the `More options` menu.
   The list of test cases allows you to look up if a test case is enabled or disabled,
   and swap its status to the opposite.
   Depending on the status of the workflow, the tab, shown in :numref:`figure-alyvix-nodes-session-workflow`,
   allows you to enable it for execution, stop or force-stop it by using the associated control.

   :numref:`figure-alyvix-nodes-multiple-sessions-workflow` shows how any of these actions can be
   performed on multiple sessions by simply
   selecting them from the node's list of sessions. Above the table, an action bar will then appear,
   enabling a user to control the workflow on all the selected sessions.

.. _figure-alyvix-nodes-session-workflow:

.. figure:: /feature-modules/alyvix/img/alyvix-session-workflow.png
   :alt: Session workflow

   Session workflow


.. _figure-alyvix-nodes-multiple-sessions-workflow:

.. figure:: /feature-modules/alyvix/img/alyvix-node-multiple-sessions-workflow.png
   :alt: Control multiple sessions workflow

   Control multiple sessions workflow

.. _alyvix-license-tab:

License Tab
+++++++++++++

The Alyvix license should be activated individually for each node.
The License tab contains the following details about the license of a particular node:

* **Status**: the status of the license: *Disabled*, *Active*, *Expiring* or *Expired*
* **Subscription plan**: the subscription plan currently in use
* **Expiration date**: the expiration date of the license
* **Remaining days**: days left before license expiration
* **Licensed sessions**: the number of sessions available with the subscription plan currently in use

A new session can only be started with the license being in an *Active* or *Expiring* status.

*Disabled* license status is usually displayed for new users that have not activated their license yet.
This can be done in the License tab. Download the request key in order to obtain a new
activation key for the selected node. Once the request key has been downloaded, please contact
your Alyvix provider in order to obtain the activation key file, which can then be uploaded directly
in the node's `License` tab.

One month prior to the end of the licensing period the license status is changed to *Expiring*.
The licensing period is defined by the contract with the Alyvix Service provider and cannot be controlled from |ne|.
In the case of an *Expiring* license please contact your Alyvix provider in order to extend your license
and be able to seamlessly use Alyvix Module.

You can alternatively contact Alyvix team at info@alyvix.com for more information.


General Settings Tab
++++++++++++++++++++

The `General Settings` tab reports the private key for the selected node and the retention period, in hours, for
the successful test cases, failed test cases and for the service logs.
Moreover, a toggle enables the capture of annotated screenshots also for successful test case runs.
You can also edit settings in the `General` tab.

.. _alyvix-timeperiods-tab:

Time periods Tab
++++++++++++++++

The `Time periods` tab reports the list of all time periods currently stored on the Alyvix Node.

In case a time period is not synced with its definition in the Director, one of the following
two statuses will be reported:

.. |timeperiod_modified_icon| image:: /feature-modules/alyvix/img/alyvix-timeperiod-modified-icon.png
   :height: 24px
   :width: 24px

.. |timeperiod_deleted_icon| image:: /feature-modules/alyvix/img/alyvix-timeperiod-deleted-icon.png
   :height: 24px
   :width: 24px

* |timeperiod_modified_icon| *Modified* (The time period is present in the Director, but its definition does not match the one of the Alyvix Node)
* |timeperiod_deleted_icon| *Deleted* (The time period was deleted from the Director but the associated Test Cases are still run based on it)

Furthermore, the :guilabel:`Sync with NetEye` button allows allows you to update the time periods present on the Alyvix Node
based on their definition in the Director.

.. note:: In case of deleted time periods, a replacement time period should be specified, using the associated
          dropdown. If you choose to apply the default replacement `Any (default)`, after the sync the Test Cases associated
          with it will be executed at all times.


.. _figure-alyvix-timeperiods-tab:

.. figure:: /feature-modules/alyvix/img/alyvix-timeperiods-tab.png
   :alt:

   The time periods stored on the Alyvix Node
