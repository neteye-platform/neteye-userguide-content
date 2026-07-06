.. _hostname-configuration:

Keycloak Hostname Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Keycloak, the component in charge of :ref:`user authentication <auth-admin-console>`, needs to know
the public hostname through which |ne| is reached in order to correctly generate redirect URLs,
issue tokens, and validate incoming requests. This is configured in
:file:`/etc/neteye-environment.yaml` and applies to all installation types.

.. _hostname-configuration-setup:

Setup
`````

Create the frontend domain configuration file from the shipped template::

   cp /usr/share/neteye/setup/neteye-environment.yaml.tpl /etc/neteye-environment.yaml

Set ``frontend_domain`` to the domain through which |ne| is accessed by users and systems:

.. code-block:: yaml

   neteye:
     frontend_domain: <your-domain>

.. warning:: On a **Cluster**, :file:`/etc/neteye-environment.yaml` is **not** stored on shared
   storage. After creating or editing it, propagate the file to all nodes::

      neteye config cluster sync

.. _hostname-configuration-fresh-install:

Fresh Install
`````````````

On a fresh install, :command:`neteye install` reads :file:`/etc/neteye-environment.yaml`
and automatically configures Keycloak with a fixed hostname
(``KC_HOSTNAME_STRICT=true``). No additional steps are required.

.. _hostname-configuration-upgrade-migration:

Post-Upgrade Migration
``````````````````````

After an **update or upgrade** from an installation that predates this setting,
Keycloak is configured with dynamic hostname resolution (``KC_HOSTNAME_STRICT=false``)
to avoid breaking the service during the update.

.. important:: Configuring a fixed hostname is a **prerequisite for upgrading to NetEye 4.50**.
   If you skip this step, the upgrade will fail.

To switch to the fixed hostname configuration, run the migration command::

   neteye cluster upgrade-prerequisites neteye-domain apply

This sets ``KC_HOSTNAME``, ``KC_HOSTNAME_STRICT=true`` and
``KC_HOSTNAME_BACKCHANNEL_DYNAMIC=true`` in Keycloak's environment, then restarts Keycloak.

.. note:: The ``apply`` command requires a valid HTTPS certificate to already be served at
   ``https://<frontend_domain>/auth``. To revert the change, use the
   :ref:`rollback <neteye-cluster-upgrade-prerequisites-neteye-domain-rollback>` command.

To verify the migration, log in to the |ne| web interface and confirm that all URLs and redirects
use the configured ``frontend_domain``.

.. _hostname-configuration-details:

How It Works
````````````

.. _hostname-strict-mode:

Hostname Strict Mode
''''''''''''''''''''

|neb| configures Keycloak differently depending on the installation path:

- On a **fresh install**, Keycloak is configured with ``KC_HOSTNAME=https://<frontend_domain>/auth``,
  ``KC_HOSTNAME_STRICT=true``, and ``KC_HOSTNAME_BACKCHANNEL_DYNAMIC=true``.
  ``KC_HOSTNAME_STRICT=true`` makes Keycloak always use the fixed public hostname, while
  ``KC_HOSTNAME_BACKCHANNEL_DYNAMIC=true`` lets internal communication (e.g. between cluster nodes)
  resolve dynamically.
- After an **update or upgrade** from an installation that predates this setting,
  ``KC_HOSTNAME_STRICT=false`` is explicitly set instead. This is necessary because an existing
  installation may not yet have ``frontend_domain`` configured or a valid certificate in place.
  With ``KC_HOSTNAME_STRICT=false``, Keycloak resolves its hostname dynamically from request headers,
  which keeps the service running after the upgrade.

Properties Mapping
''''''''''''''''''

|neb| sets these values as environment variables in :file:`/neteye/shared/keycloak/conf/keycloak.env`,
which is equivalent to the following Keycloak native configuration properties in
:file:`/neteye/shared/keycloak/conf/keycloak.conf`:

.. code-block:: properties

   hostname=https://<frontend_domain>/auth
   hostname-strict=true
   hostname-backchannel-dynamic=true
   http-relative-path=/auth

.. note:: ``http-relative-path=/auth`` is already statically configured in :file:`keycloak.conf`
   and is the reason why the hostname URL always ends in ``/auth``.
