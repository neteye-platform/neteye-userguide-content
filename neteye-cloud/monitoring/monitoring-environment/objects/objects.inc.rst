With the |nec| Monitoring service you can monitor the availability of your hosts
and services using check commands.

Monitored Objects
~~~~~~~~~~~~~~~~~

There are two main types of objects that monitoring works with:

* **Hosts:** Physical or virtual devices that correspond to a "computer". They
  have a CPU, memory and storage (which can be measured), and run processes
  launched by the user or the system itself. Hosts can be "Up", "Down" or
  *Unreachable*, depending on whether they're functioning or not. Examples include:

  * Computers (servers, laptops and tablets)
  * Virtual machines
  * Network devices (switches, routers)
  * IT infrastructure (security alarms, smart locks)
  * IoT devices (temperature sensors, water detector)

  Host objects provide a mechanism to group services that are running on the same
  physical device.

* **Services:** The software programs and processes that run on hosts, like
  Active Directory,
  or network services (HTTP/S, SNMP, SSH).  They run faster or slower depending
  on the current performance of their host (as long as their host is "Up"), and
  even crash and need to be restarted.

  The service states are:

  * *OK:* The service is working correctly
  * *WARNING:* The service is working, but its performance has degraded past
    a certain, definable point
  * *CRITICAL:* The service's performance is highly degraded, for instance it
    responds slowly to user input
  * *UNKNOWN:* The service's state can't be determined (for example, it
    doesn't respond)

Modern host operating systems will typically include one or more services that
are dedicated to monitoring, such as reporting CPU load, memory or storage
capacity used, and network throughput. These are called
`check commands <https://icinga.com/docs/icinga-2/latest/doc/10-icinga-template-library/#check-commands>`__,
and they return the service state and other result values.  User-provided
check commands called *plugins* can also be installed.

We can check the status and performance of a service at any given moment by
looking at the check results using:

* *Views*, which show the most recent return values
* *Dashboards*, which show multiple views as panels in a larger screen
* *Interactive Dashboards*, which continually store those check results
  and let you compare them with previously recorded values


Host and Service Templates
``````````````````````````

Because there can be many different types of hosts that need different check
commands and other parameters, it's more efficient to implement an inheritance
system of templates than repeatedly re-enter the same information for each
host.

Templates allows you to quickly define a new host by choosing the most
relevant template and then only making the few necessary changes to
differentiate that host from others, such as host name and IP address.

The same logic also applies to services (service templates) and commands
(command templates).  You can find more details about all three templates in
the On-Premise version of the `NetEye Guide <https://neteye.guide/current/core-modules/director/templates.html>`__.

Within |nec| there are already many pre-defined templates to use, so it's
more important to learn what they are and how they are used rather than how
to create new ones.


How to Add a Host or Service
````````````````````````````

If you need to add a specific host, command or service, that information
can also be found in the On-Premise |ne| guide:

* `Hosts <https://neteye.guide/current/core-modules/director/objects.html#adding-a-host>`__
* `Commands <https://neteye.guide/current/core-modules/director/objects.html#adding-a-command-check>`__
* `Services <https://neteye.guide/current/core-modules/director/objects.html#adding-a-service>`__


How to See Your Hosts and Services
``````````````````````````````````

In |nec| you can view all the hosts in your monitored environment by following
:menuselection:`Overview > Hosts` in the left side panel.  Each host is
displayed with a brief summary including:

* Its name
* Its status
* How long the current state has been that way
* Additional check or plugin output beyond the state

If a host is down it will be highlighted in red so it is quickly noticed.

When you have a large number of hosts they cannot all be displayed on one page,
|nec| provides page controls that let you determine how many hosts to show on
one page, how to navigate from one page to another, sort the hosts in the list,
and quickly search for hosts by name.

You can see the list of your services just like the host list by following
:menuselection:`Overview > Services`.


Check Commands and Plugins
``````````````````````````

A *plugin* is the monitoring code that exists on the monitored object that
actually computes the result and returns the desired value.  The check command
runs when directed on the |nec| side and calls the plugin via a network
connection to request the check and to collect the returned value(s). A check
command and plugin working together are typically just called a *check*.

Although you can write your own commands, most of the time you will just select
one that already exists in |nec|, either internally derived from Icinga's
library, or from a third party library, called *external commands*. Both types
typically have a large number of arguments you can change (or leave blank)
directly in |nec|.

.. _figure-nec-commands-and-external:

.. figure:: /neteye-cloud/monitoring/img/command_and_external.png
   :alt: Comparison of commands and external commands

   Commands on the left, External Commands on the right

Each command has a name, and the **Arguments** tab (or **Preview** tab for
external commands) shows the relationship between the command line arguments
and |nec|/Icinga variables you can use to populate them.

To select and use an external command, you can search online to find out how
to install the associated plugin on the host device and use the arguments to
obtain the check values you're interested in.


Reachability and Dependencies
`````````````````````````````

A *reachable* host is one that can "answer" if you talk to it, and an
*unreachable* host doesn't answer. This is determined by using a command
called *hostalive* (ping) that sends a network request to the host. If the
request is answered, the host is reachable at that moment.

But a host may not answer either because (1) it's down or (2) it's up but
the network between the host and the monitor is not working.  It's not always
easy to tell the difference. For instance, if other hosts on the same subnet
are reachable, then the host is likely down.  If they aren't, it's more
likely a network problem.

When a host is down, any services running on that host will also not be working.
And any other hosts that need to access services on the first host will have
problems as well.  These *dependencies* can be stated explicitly so that when
a "parent" host is marked down in the monitoring interface, any "child" hosts
and services that depend on it can be immediately marked as *Unreachable*.

.. _figure-nec-unreachable-host:

.. figure:: /neteye-cloud/monitoring/img/unreachable_host.png
   :alt: An unreachable host
   :width: 397px
   :align: center

   A host marked as *Unreachable*

Services can also be marked as unreachable.  In fact, services can serve as
both parents and children of either other hosts or other services.

Dependencies can especially help root cause analysis of a problem, because if
a large number of hosts or services stop working, you can concentrate first on
the "Down" objects before the "Unreachable" ones.

It can also mitigate the chaos when there are many dependent hosts:  Child
objects can be set to suppress notifications and further checks, so that a
single problem doesn't result in being inundated by tens of thousands of
messages.
