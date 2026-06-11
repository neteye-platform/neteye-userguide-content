

.. _monitor-network-advanced:

Like for all other modules, the *Full Module Access* and *General Module
Access* permissions are available for the ntopng module, mapped to the
**Administrator** and **Non Privileged user** roles in ntopng,
respectively. An Administrator will have full access to all the modules'
functionalities and will not be subject to the *restrictions* listed
below.

An additional Permission is peculiar to this module, namely
*pcap-download*, that allows download
`.pcap <https://en.wikipedia.org/wiki/Pcap>`__ file captured by ntopng.

There are two *Restrictions* available for this module:

-  **allowed-interfaces** is used to mark which interfaces are available
   to the role. Wildcard can be used, default is ``tcp://*:5556c``. The
   trailing ``c`` is important as it lets ntopng act as a **c**\ ollector.
-  **allowed-networks** allows access only to those flows originating
   from the given (local) networks

.. note:: Restriction can be applied to only one ntopng interface at time. For this
   reason, if a user has multiple roles and each role has an interface set in the
   restriction, the user will **only** be able to see **the first interface**.f
   In case the wildcard ``*`` is set in at least one of the roles,
   the user will be able to see **all the interfaces**.
