The **RDP Client in the Browser** opens a remote desktop connection to a
session of an Alyvix node directly from the |ne| web interface. You do not need
to install an RDP client on your workstation, nor to look up the connection
details of the session.

.. note:: The feature works only for sessions on nodes reachable from the
    |ne| master node. To connect via RDP to other nodes, please refer to the
    :ref:`local RDP client <alyvix_rdp_monitor_local_client>`.

Prerequisites
`````````````

To use this feature, make sure that:

- You have the :ref:`administrator <alyvix_permissions_roles>` role on the node:
  you are either a |ne| super administrator, or a Tenant administrator of the
  Tenant to which the node belongs. On a
  :ref:`Tenant shared node <alyvix_multitenant_tenant_shared_node>`, only super
  administrators can connect. If you are not an administrator of the node, the
  **Connect to RDP** button is disabled.

- You know the password for the Windows session. |ne| does not store it for
  this purpose, so you will enter it at every connection.

- The RDP certificate of the node is signed by a Certificate Authority that
  ``guacd`` trusts, see :ref:`alyvix_rdp_certificate`.

.. _alyvix_rdp_certificate:

Configuring the RDP Certificate of a Node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``guacd`` verifies the RDP certificate of the node before it connects. If the
certificate is not signed by a Certificate Authority (CA) that ``guacd`` trusts, the
connection fails with an :ref:`SSL/TLS error <alyvix_rdp_browser_troubleshooting>`.
To make ``guacd`` trust the certificate:

#. Get the public certificate of the CA that issued the RDP certificate of the
   node. If the RDP certificate is self-signed, get the certificate of the node
   itself.

   .. note:: The RDP certificate must be valid for the address set in the
      Director host that represents the Alyvix node, because ``guacd`` uses that
      address to connect to the node.

#. Copy the certificate in the directory
   :file:`/neteye/local/rke2/conf/ca-trust/`, with the :file:`.crt` extension.
   On a |ne| cluster, copy it to **every node**: the directory must have the
   same content on all the nodes, otherwise the next step stops with an error.

#. Apply the change by running the :ref:`neteye install command <neteye-install>`
   for the |ne| Operator only:

   .. code-block:: console

      # neteye install --restrict-services-to neteye-operator

   The command merges all the certificates of the directory, together with the
   |ne| root CA, into one CA bundle. If the bundle changed, ``guacd`` restarts
   and starts to trust the new certificate.

   .. note:: The command also checks the |ne| Operator against its channel:
      if a newer version of the Operator is available for your |ne| release,
      the command updates it as well.

Opening a Connection
````````````````````

To connect to a session:

1. Open the Alyvix UI and go to the :ref:`Nodes page <alyvix_nodes_list>`.
2. Select the node, then open the :ref:`Sessions tab <alyvix_sessions_tab>`.
3. Open the :ref:`live preview <alyvix_figure_alyvix_live_preview>` of the
   session you want to reach and click **Connect to RDP**. The RDP
   client opens in a new browser tab.
4. In the **Remote Desktop** window, check the host name and enter the
   credentials of the Windows session:

   - **User name**: found in the Session tab of the Node details page (automatically filled).
   - **Password**: the password of the session.

5. Click **Log in**.

.. _figure_alyvix_connect_to_rdp:

.. figure:: /feature-modules/alyvix/img/connect-to-rdp.png
   :alt: Connect to RDP button

   The **Connect to RDP** button in the Sessions tab of the Node details page.

When the connection is established, the remote screen appears with a green
border and the toolbar at the top. If the page cannot connect, it shows one of
the messages in :ref:`alyvix_rdp_browser_troubleshooting`.


The RDP Session Page
````````````````````

The RDP session page shows the remote screen of the session. A toolbar floats
over the remote screen and includes the following buttons.

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - Button
     - Description
   * - **Fit** / **Real**
     - Opens the **Picture size** menu. **Fit** scales the remote screen
       to the page, keeping its aspect ratio. **Real** shows it at its
       real resolution, with scrollbars if it is larger than the page.
   * - **Keyboard**
     - Shows or hides the on-screen keyboard, which sends keystrokes to the
       session that the physical keyboard cannot send, for example
       :kbd:`Ctrl+Alt+Del` or :kbd:`Win`. Drag it anywhere on the page:
       it reopens where you left it.
   * - **Full screen**
     - Shows the page in full screen, without the browser. The button changes
       to **Exit full screen**.
   * - **Hide UI**
     - Collapses the toolbar, so it does not cover the remote screen. A small
       button brings it back.
   * - **Disconnect**
     - Closes the RDP connection, after you confirm.

.. _figure_alyvix_rdp_session_page:

.. figure:: /feature-modules/alyvix/img/rdp-session-page.png
   :alt: RDP session page

   The RDP session page showing the remote screen and the toolbar.

How It Works
````````````

The browser cannot speak the RDP protocol, so the connection goes through
`Apache Guacamole <https://guacamole.apache.org/>`_, a clientless remote desktop
gateway that runs in the RKE2 cluster of |ne|. When you click
**Connect to RDP**:

#. The RDP session page asks the Alyvix module for a connection. The module
   checks that you are an administrator of the node and asks Guacamole for a
   token that is valid for one connection only.
#. The page uses the token to connect to the Apache HTTP Server of |ne| over a
   secure WebSocket. Apache forwards the token to Guacamole,
   through the Gateway of the Tenant.
#. You enter the credentials of the Windows session in the **Remote Desktop**
   window. They go directly to ``guacd``, the Guacamole service that handles the
   remote connection, and |ne| does not store them.
#. ``guacd`` connects to the Alyvix node through its RDP port, 3389 by default, and
   translates between the Guacamole protocol of the browser and the RDP
   protocol of the node.
#. The page shows the remote screen and sends your mouse and keyboard inputs
   to the session.

.. warning:: A connection to a session is a real Windows login. While you are
   connected, your mouse and keyboard actions reach the session in which Alyvix
   runs the test cases, and **they can make the test cases fail**. Connect only
   when you need to, and :ref:`disconnect <alyvix_rdp_browser_disconnect>` when
   you are done.

.. _alyvix_rdp_browser_troubleshooting:

Troubleshooting
```````````````

.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Message
     - What to do
   * - **The browser blocked the RDP page.**
     - Your browser or an extension blocked the new tab. Allow pop-ups for |ne|
       and click **Connect to RDP** again.
   * - **Windows login failed**
     - The user name or password is incorrect. Click
       **Reconnect** and enter the credentials again.
   * - **RDP server closed/refused connection: SSL/TLS connection failed
       (untrusted/self-signed certificate?)**
     - The RDP certificate of the node is not signed by a Certificate
       Authority that ``guacd`` trusts, or it is not valid for the address
       specified in the Director host representing the Alyvix node.
       See :ref:`alyvix_rdp_certificate`.
   * - **Cannot connect to the RDP gateway.**
     - The RDP gateway does not respond. Try again after a few moments. If the
       problem persists, contact |support|_.
   * - **Cannot get the connection data for this session.**
     - |ne| could not prepare the connection, for example because the node does
       not respond. Check the state of the node in the Nodes page, then try
       again.
