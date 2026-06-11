.. _elastic-agent-configuration:

Configuring Elastic Agent
~~~~~~~~~~~~~~~~~~~~~~~~~

Elastic Agent comes out-of-the-box in |ne| installations: every |ne| node hosts a running Elastic Agent with a
preconfigured policy that depends on the role of the node in the |ne| system. Since the Elastic Agent implementation
is officially supported, the Elastic Agents running on |ne| nodes should be assigned to the default |ne| policies
and not to other custom policies.

In order to customize the policy parameters and the |ne|-managed integrations with the ``-neteye`` suffix:

- Customize the configuration files in :file:`/neteye/local/elastic-agent/conf/fleet/templates/` specifying the parameters
  you want to change.

- Apply the new configuration on all the |ne| nodes by running the following command on the node where the configuration
  file has been modified:

  .. code::

    neteye tenant config apply master

  This will synchronize the changes in the configuration file and apply the customization to the enrolled Elastic Agents
  in the system.

.. _elastic-agent-fleet-server-config:

Fleet Server
````````````

By default, Fleet Server is exposed, through Nginx, using the certificate and
key located at :file:`/neteye/shared/nginx/conf/certs/fleet-server-external.crt.pem`
and :file:`/neteye/shared/nginx/conf/certs/private/fleet-server-external.key`.

The certificate and key are automatically generated upon |ne| first installation
procedure, however it is recommended to install a new trusted certificate as
soon as possible.

.. warning::
   Please take note that in order to allow the correct communication between
   Elastic Agents and the Fleet Server, the *full chain* of certificates should be
   included in the certificate file.

Moreover, the cryptographic settings applied to the SSL connections replicate the settings applied at a system level,
which may be changed as described in the `official RHEL documentation <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening>`__.

.. warning:: The following policies in Fleet should not be modified by customers in order to avoid
   any kind of issue during update/upgrade procedures: NetEye Operative Nodes, NetEye Single-Purpose Nodes.
   Please remember, that even though integrations can be added, the settings of these policies should not be modified.



.. _elastic-agent-on-satellites:

Elastic Agent on Satellites
```````````````````````````

The installation of Elastic Agents on |ne| Satellites is not yet officially managed by the |ne| system. Elastic
Agent can still be installed and configured manually in order to obtain a Tenant-related Agent.

The following guidelines will explain how to correctly configure an Elastic Agent on a |ne| Satellite:

- Before adding the Elastic Agent to the |ne| system, a dedicated Policy is needed for the Satellite: Through
  the "Fleet" section in Kibana, an additional Policy called ``<tenant_display_name> Satellite`` can be created,
  using ``<tenant_id>`` as namespace for the Policy and the associated integrations.

- After the installation of Elastic Agent from the |ne| repositories has been completed, the Agent can be enrolled
  to Elastic using the Fleet Server of the |ne| Master, reachable at ``<neteye_master_fqdn>:8220``.

- The Elastic Agent can use the already configured Output ``neteye-external-output``, connected to the Elasticsearch
  server on the |ne| Master.

- Once the Elastic Agent has been enrolled in the |ne| Fleet, additional integrations can be added and
  applied to the Tenant-related Elastic Agent running on the |ne| Satellite. Every integration belonging
  to the previously created Policy, associated with a specific Tenant, should have the ``-<tenant_id>`` suffix
  in order to maintain a good separation among different Tenants.

  For example, a good choice for the APM integration of a Tenant having ``tenant_id = 12345`` could be ``apm-12345``

.. _signing-elastic-agent-documents:

Signing Elastic Agent Logs
``````````````````````````

In order to sign documents collected by any Elastic Agent in the |ne| environment using El Proxy, it is possible to
configure Elastic Agent to send logs to Logstash for the signing process. Once El Proxy has been correctly configured
and enabled as described in :ref:`enabling-el-proxy`, the subsequent sections can be followed in order to apply the
correct configuration to the specific scenario:

.. note::

    If your Elastic Agents need to connect to the NetEye Logstash using an hostname different than the |ne|
    Hostname (declared in :file:`/etc/neteye-cluster` for Clusters and ``hostname --fqdn`` for Single Nodes)
    in the case they run outside the company network, for example, you can substitute the certificate
    :file:`/neteye/shared/logstash/conf/certs/logstash-server-external.crt.pem` and private key
    :file:`/neteye/shared/logstash/conf/certs/private/logstash-server-external.key` with trusted certificates
    valid for the needed hostname.

Elastic Agent on the Master Tenant
++++++++++++++++++++++++++++++++++

If Elastic Agent belongs to the :term:`Master Tenant`, it is enough to change the output of the Elastic Agent and
send the logs to be signed to Logstash instead of sending them directly to Elasticsearch.

This operation can be executed under :menuselection:`Fleet / Agent Policies / <selected_policy> / Settings` by changing the
`Output for integrations` field to the preconfigured ``neteye-external-logstash-output`` output.

Note that **<selected_policy>** should be the policy assigned to the Elastic Agent that will then send the logs to be
signed by El Proxy. Every other Elastic Agent connected to the same policy will change its output, starting to
send logs to Logstash.

Elastic Agent on other Tenants
++++++++++++++++++++++++++++++

In the case Elastic Agent belongs to a Tenant different to the :term:`Master Tenant`, the Elastic Agent should
send the collected logs to the Logstash instance running on the |ne| Satellite of its Tenant.

In order to change the output of the Elastic Agent that should send logs to be signed, an additional output should be
created in :menuselection:`Fleet / Settings / Outputs` defining as ``Logstash`` the output type of the configuration and filling the
other parameters of the form.

Since the documents should be sent to the Logstash instance of the |ne| Satellite, the Satellite FQDN and the port
5045 should be specified as `Logstash Host`, for example ``<satellite_fqdn>:5045``.

Note that the |ne| instance of Logstash already has a preconfigured pipeline dedicated to Elastic Agent input sources,
no additional configurations are required in Logstash.

Once the new output options has been stored in Fleet, it is possible to assign the newly created output to the policy
of the Elastic Agent that is collecting logs to be signed.
