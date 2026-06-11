.. _selfmonitoring:

Self Monitoring
~~~~~~~~~~~~~~~
In order to provide high-quality monitoring experience, it is important to always make sure
the NetEye is functioning the way it should. Thus, just as NetEye monitors the states of hosts and services,
it is also important to monitor the health of NetEye itself.

For this purpose NetEye provides several options to perform self monitoring, and here
NetEye relies on two mechanisms:

#. a special ``neteye-local`` host to be monitored
#. a number of health checks that are carried out on that host

There are multiple reasons for monitoring NetEye's health, and so,
depending on the scenario, you may run *light* or *deep* checks.

While a light check is a quick way to learn whether important parts of NetEye
are up and running, deep checks are intended for tasks like verifying the integrity
and consistency of resources.

Check out the dedicated :ref:`self-monitoring-module` section to learn more about how to create a default host/service check
or run health checks.
