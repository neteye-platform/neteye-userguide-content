.. _passive_monitoring:

Passive Monitoring
~~~~~~~~~~~~~~~~~~

|ne|'s monitoring strategy incorporates both Active and Passive Monitoring
approaches. While Active Monitoring allows you to proactively monitor your infrastructure,
a solution that performs Passive Monitoring creates a complete view of
your infrastructure state, performance, and behavior, without actively interacting with it.

In the process of Passive Monitoring, |ne| will analyze the data received
from your devices, process it and then, if required, generate and execute actions
based on your needs and preferences.

Passive Monitoring proves to be useful when monitored devices and infrastructure do not support
an agent for Active Monitoring, and it is possible to tune them
to send particular events to |ne| for subsequent processing.

Passive Monitoring is more resource-efficient compared to Active Monitoring. It consumes
minimal resources as it works with existing data flows. Additionally, it doesn't introduce
additional traffic or load. This makes it perfectly suitable for critical production environments
where minimizing disruption is crucial.

To assist in running Passive Monitoring processes, i.e. receiving data from various sources
without implementing custom processors, :ref:`Tornado <tornado-architecture>` software was
integrated into |ne|.

Tornado, a Complex Event Processor, receives reports of events from data sources such
as monitoring, email, and telegram, matches them against
pre-configured rules, and executes the actions associated with those
rules. These may include sending notifications, logging to files, and
annotating events in a time series graphing system.
