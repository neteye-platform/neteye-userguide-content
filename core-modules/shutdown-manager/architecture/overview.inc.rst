
Orchestrated Datacenter Shutdown
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Shutting down large numbers of hosts can be very useful in the event of
emergencies like fire or extended power loss. Shutting down servers in
an orderly manner can prevent data loss and speed up recovery time.

The Shutdown Manager lets you define one or more shutdown groups, either
manually, or automatically when triggered by a condition based on
monitoring results. For a higher efficiency, groups have a declared
order in which they will be turned off. Hosts within the same group will
be turned off concurrently, while subsequent groups will only be shut
down after a specified amount of time has elapsed after the previous
group shutdown had started.

Architecture of the Shutdown Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Shutdown Manager module is organised on three levels: the top level
is called **Shutdown Definition** and contains a definition of the
actions to be taken; a **Shutdown Group**, which consists of a group of
hosts that will be acted upon in the same time-span; and a **Shutdown
Host** which consists of the single hosts on which the shutdown action
will be carried out.

-  Each **Shutdown Definition** contains three main information: a
   **name**, a **condition** on either a host or a service, and a **list
   of groups**. It must then be deployed in Tornado, writing a rule that
   verifies the shutdown condition; afterwards, whenever the condition
   is met, a shutdown is scheduled on the groups. As an example,
   consider a host connected to a sensor which monitors the temperature
   in the server room. The condition can be a high temperature, and the
   groups to be shutdown contain the servers in the room, according to
   the order in which they must be turned off.

-  The **Shoutdown Group** contains the list of hosts that will be
   processed in parallel and a few information about them, and the
   **timeout**; i.e., the amount of time after which the next group will
   be processed.

-  The **Shoutdown Host** consists of an host and of the commands that
   will be executed on it during when a shutdown is invoked. The command
   definition might include variables, i.e., placeholders that will be
   replaced with suitable values when they are executed on each host.

The following sections describe the components comprising the Shutdown
Manager and how to use the CLI to configure automatic shutdown scenarios
and manually shut down groups and individual hosts.
