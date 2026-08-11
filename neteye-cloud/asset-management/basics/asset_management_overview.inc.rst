
.. _asset-management-on-cloud:

Asset Management in |nec|
~~~~~~~~~~~~~~~~~~~~~~~~~

Assets & IT Service Management is a functional module of the |nec| platform.

It provides a centralized view of IT assets as part of an infrastructure observability approach.
This allows you to maintain visibility over your organizations' infrastructure components, understand
where assets are used, and relate them to services, security controls, and compliance requirements.

Asset Management is used to track IT and office equipment across the organization,
supporting operational awareness and infrastructure understanding in a single consolidated system.

GLPI Integration
````````````````

The Asset Management service is based on GLPI, a web application designed for IT asset and service management.

GLPI provides capabilities for:

- Hardware inventory management
- Software inventory and tracking
- Centralized IT asset organization
- IT service and user support processes

As a cloud-delivered service, GLPI is accessed through the |nec| platform and does not require local installation
or system maintenance by the customer.

Asset Inventory
```````````````

GLPI allows organizations to maintain a structured inventory of IT assets used within their environment.
This includes servers, workstations, network equipment, and other infrastructure components.

Assets can be:

- Registered manually
- Collected automatically via inventory mechanisms
- Continuously synchronized from managed devices

This ensures that the asset database remains up to date and reflects the actual state of
the infrastructure.

GLPI Agent
``````````

Asset information within the platform can be collected from managed devices using the GLPI Agent.

The GLPI Agent is a lightweight component installed on endpoints such as workstations, servers,
or other managed infrastructure devices. It gathers technical information about the system and its software and
securely sends this information to the GLPI platform, where it becomes part of the centralized
asset inventory.

In internal network environments, agents typically communicate with the GLPI server through
the local network.

However, for devices located outside the internal network, communication can be established
through a satellite endpoint. In this model:

- The agent connects to a Satellite endpoint exposed externally
- Communication uses HTTPS over port 443/TCP
- Each Satellite is identified by a fully qualified domain name (FQDN)
- Secure communication is ensured using a public certificate provided by |witit| with Satellite configuration

This architecture allows devices to report asset information to the platform regardless of
their physical location, while maintaining secure connectivity to the Cloud environment.

To ensure reliable operation, organizations should verify that managed devices are able
to reach the Satellite endpoint from their network environments.

Cloud Deployment Characteristics
````````````````````````````````

When delivered through |nec|, Asset Management operates as a fully managed service.
The underlying GLPI platform, including infrastructure, updates, and maintenance activities,
is handled at the service level.

This removes the need for you to manage installation, upgrades, or infrastructure components
such as servers and databases.

The Cloud deployment also provides a consistent operational model across environments,
consolidating asset data from both internal and remote devices into a single managed platform.
This simplifies asset visibility in distributed infrastructures and avoids the need for separate
GLPI installations or custom synchronization setups.
