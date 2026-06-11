
Agents
~~~~~~

Below is the list of agents that will need to be installed
on the hosts defined within the perimeter and their compatibility matrices.

Elastic Agent
`````````````

Elastic Agent is installed with the relevant integrations, depending on the role of the host
on which the installation takes place.

This agent is used to collect events and send them to the SIEM.
`Here <https://www.elastic.co/support/matrix/>`__ you can view the compatibility matrix of the Elastic Agent.

**Install on**: all Microsoft Windows and Linux servers.


Icinga Agent
````````````

Icinga Agent is used to monitor the situation at the operational level of the various hosts
and to send any active commands to the hosts.

`Here <https://icinga.com/subscription/support-matrix/>`__ you can view the compatibility matrix of the Icinga Agent.

**Install on**: all Microsoft Windows and Linux servers.


Sysmon
``````
This official `Microsoft component <https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon>`__ is installed
on Windows servers and makes it possible to significantly improve the generation of events
at the operating system level, making it possible to implement valid detection logics
even in the event of bypassing the EDR solution that may be in use.

Events are then collected by the Elastic Agent and sent to the SIEM.

**Install on**: all Microsoft Windows and Linux servers.
