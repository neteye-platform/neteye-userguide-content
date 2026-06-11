
APM Configuration
~~~~~~~~~~~~~~~~~

The APM functionality is already configured to work out-of-the-box in |ne|
installations if all the listed :ref:`requirements<individual-module-reqs>` are
satisfied. The APM Server is exposed through Nginx using the certificate and key located at
:file:`/neteye/shared/nginx/conf/certs/apm-server-external.crt.pem` and
:file:`/neteye/shared/nginx/conf/certs/private/apm-server-external.key`.

The certificate and key are automatically generated upon |ne| first installation
procedure, however it is recommended to install a new trusted certificate as
soon as possible.

.. warning::
   Please take note that in order to allow the correct communication between
   Elastic Agents and the APM Server, the *full chain* of certificates should be
   included in the certificate file.

Moreover, the cryptographic settings applied to the SSL connections replicate the settings applied
at a system level, which may be changed as described in the
`official RHEL documentation <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening>`__.

Since APM Server is running as an :ref:`Elastic Agent<elastic-agent>` integration,
:ref:`this<elastic-agent-configuration>` procedure can be followed in order to customize the
APM Server configuration as part of the Elastic Agent configuration settings.

Collecting APM Data
```````````````````

In order to collect APM Data, the following two components are required:

- An **APM Agent**, that will measure APM data and traces from a specific target. If additional APM Agents are
  required, the official Elastic guide about `APM Agents <https://www.elastic.co/guide/en/apm/agent/index.html>`__
  can be followed.

- An **APM Server**, in charge of collecting the APM Data from the related Agents.

The **APM Server** installation varies depending on the target tenant from which APM data are collected.
The following sections will explain the configuration of the two main scenarios of collecting APM data:

From the Master Tenant
++++++++++++++++++++++

By default, |ne| comes with a preconfigured APM Server running in the Operatives nodes and collecting |ne|
Master related APM data. The default implementation works out-of-the-box without the need for any additional
configuration, assigning the collected data to the ``master`` namespace.

Then, to collect APM data from the Master Tenant using additional APM Agents, simply configure the APM Agent
on your target application and set as output the FQDN of your NetEye Master.

Please also notice that the NetEye APM Server integration by default requires API key authentication from
the agents. To create and set the API key in your APM agent you can refer to
`Create an API key in the APM app <https://www.elastic.co/docs/solutions/observability/apm/api-keys#apm-create-an-api-key>`__.

From other Tenants
++++++++++++++++++

In order to collect APM data from other tenants, a |ne| Satellite is required. Since |ne| currently does not
officially support this scenario, you can follow the guidelines listed in the section dedicated to the
:ref:`Elastic Agent Configuration on Satellites<elastic-agent-on-satellites>` in order to collect APM data
from a specific tenant. The linked guide will explain how to configure an Elastic Agent, installed on the |ne|
Satellite, on which an APM Server integration can be added in order to fulfill this requirement.

To configure a secure connection for the APM data transmission, the creation of an API key for the APM Agent
authentication is strongly suggested. You can configure a dedicated API Key through the user interface of Kibana.
