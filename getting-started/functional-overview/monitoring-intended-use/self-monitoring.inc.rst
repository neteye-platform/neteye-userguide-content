.. _selfmonitoring:

Self Monitoring
~~~~~~~~~~~~~~~

In order to provide a high-quality monitoring experience, it is important to always ensure that
|ne| is functioning the way it should. Thus, just as |ne| monitors the state of hosts and services,
it is important to also monitor the health of |ne| itself.

For this purpose |ne| provides several options to perform Self Monitoring. Here, |ne| relies on two mechanisms:

#. a special ``neteye-local`` host to be monitored
#. a number of health checks that are carried out on that host

There are multiple reasons for monitoring |ne|'s health so that,
depending on the scenario, you may run *light* or *deep* checks.

While a light check is a quick way to learn whether important parts of |ne|
are up and running, deep checks are intended for tasks like verifying the integrity
and consistency of resources.

Check out the dedicated :ref:`self-monitoring-module` section to learn more about how to create a default
host/service check or run health checks.
