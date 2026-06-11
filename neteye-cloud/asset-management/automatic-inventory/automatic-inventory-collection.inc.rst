
.. _automatic_inventory_collection:

Automatic Inventory Collection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Automatic inventory collection allows the platform to build and maintain an up-to-date list
of assets without requiring manual registration.

This is achieved using the GLPI Agent (installed on Satellite or on a client/server host) and built-in discovery mechanisms, which scan the environment,
identify devices, and collect relevant technical information.

The functionality supports:

- Discovery of devices present in the network
- Collection of detailed asset information
- Continuous synchronization of inventory data

Discovery Methods
~~~~~~~~~~~~~~~~~

Automatic inventory collection relies on two main discovery approaches:

- SNMP-based network discovery
- Virtual environment discovery (VMware)

These methods allow coverage of both physical infrastructure and virtualized environments.

SNMP Network Discovery
``````````````````````

SNMP discovery is used to identify devices available within the network. A network discovery task
scans IP ranges and detects reachable devices using SNMP.

This allows identification of infrastructure components such as:

- Network printers
- Switches
- Routers
- Other SNMP-enabled devices

The result of this process is a list of detected devices, which are then added to the asset inventory
as known assets.

SNMP Network Inventory
``````````````````````

Once devices are discovered and added to the inventory, additional information can be collected
from them.

A network inventory task retrieves detailed data from SNMP-enabled devices already known
to the system.

This includes:

- Device type and model
- Network interfaces
- Configuration-related information (depending on device capabilities)

As a requirement, the device must already exist in the asset list, and valid SNMP credentials
must be provided.

This step enriches the asset data and provides a more complete view of network devices.

VMware Environment Discovery
````````````````````````````

For virtualized environments, inventory collection is performed using VMware APIs.

The system can connect to a VMware vCenter instance and retrieve information about the virtual
infrastructure.

This includes:

- ESXi hosts managed by vCenter
- Virtual machines deployed in the environment
- Configuration of the VMware infrastructure

This method provides visibility into virtual infrastructure and virtual machines,
while detailed guest-level inventory typically still requires an agent inside the VM.

Resulting Asset Visibility
``````````````````````````

By combining these discovery and inventory methods, the platform enables:

- Identification of virtual infrastructure components
- Collection of installed software and system information, granted the Agent is installed
- Automated updates of asset data through repeated scans and agent communication
