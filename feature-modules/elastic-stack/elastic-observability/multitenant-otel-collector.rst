
Multitenant OpenTelemetry Collector
-----------------------------------

|ne| offers an OpenTelemetry Collector component that can be used to collect and forward telemetry data to the Elastic Stack, 
exploiting OAuth authentication features to support multitenancy. 

This component is deployed on the |ne| Master nodes in Kubernetes and is configured to listen on port `4317` for incoming telemetry data.

The collector is useful when you want to collect telemetry data from multiple tenants and forward it to the Elastic Stack, without the need to deploy a separate collector for each tenant. 

This allows for a more efficient use of resources and simplifies the management of telemetry data collection.

The collector is configured to use OAuth authentication to ensure that only authorized tenants can send telemetry data to the Elastic Stack. 

To enable a tenant to exploit the collector, you need to enable the `neteye-elastic-stack` feature module in the target tenant. 

.. code-block:: bash
  
   neteye tenant config modify --enable-module neteye-elastic-stack <tenant_name>

At that point, you can login in the Keycloak admin console from the |ne| web interface, using the :menuselection:`Configuration / Authentication` menu,
and retrieve the client credentials under :menuselection:`Clients / <tenant-id> / Credentials` menu. 

The credentials can be used to send authenticated telemetry data to the collector, which will forward it to the Elastic Stack.

For example, if the sender is itself an OpenTelemetry Collector deployed in the customer environment, you can configure it to use the `otlp` exporter 
with the following extension to automatically manage the OAuth authentication and send telemetry data to the |ne| collector:

.. code-block:: yaml

   extensions:
    oauth2client:
        client_id: <client_id>
        client_secret: <client_secret>
        token_url: https://<neteye_frontend_url>/auth/realms/master/protocol/openid-connect/token
        scopes: ["api.metrics"]
        endpoint_params:
            audience: account


Otherwise, any application that supports the OpenTelemetry protocol and OAuth authentication can be configured to send telemetry data to the collector, using the same credentials and endpoint.