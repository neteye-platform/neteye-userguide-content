.. _passive_monitoring:

Passive Monitoring
~~~~~~~~~~~~~~~~~~
NetEye's monitoring strategy incorporates both Passive and Active monitoring
approaches. While Active monitoring allows to proactively monitor your infrastructure,
a solution provided for performing Passive monitoring creates a complete view of
your infrastructure state, performance and behavior without actively interacting with it.

In the process of passive monitoring, NetEye will analyze the data received
from your devices, process it and then, if required, will generate and execute actions
based on your needs and preference.

Passive monitoring proves to be useful when monitored devices and infrastructure are not supporting
the installation of an agent for active monitoring, and it is possible to tune them
to send particular events to the NetEye for the following processing.

Passive monitoring is more resource-efficient compared to active monitoring. It consumes
minimal resources as it works with existing data flows. Additionally, it doesn't introduce
additional traffic or load. This makes it perfectly suitable for critical production environments
where minimizing disruptions is crucial.

To assist in running passive monitoring processes, i.e. receiving data from various sources
without implementing custom processors, :ref:`Tornado <tornado-architecture>` software was
integrated into NetEye.

Tornado, a Complex Event Processor, receives reports of events from data sources such
as monitoring, email, and telegram, matches them against
pre-configured rules, and executes the actions associated with those
rules. These may include sending notifications, logging to files, and
annotating events in a time series graphing system.
