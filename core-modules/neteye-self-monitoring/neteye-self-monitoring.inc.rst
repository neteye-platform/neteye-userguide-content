
.. _neteye-check-creation:

Default Host and Service Check Creation
```````````````````````````````````````

As part of the initial installation, NetEye 4 creates a default host
(``HOST_NAME="neteye-local"``) that represents the NetEye monitoring
system itself. This host serves as a vehicle for conducting default
service checks on NetEye itself to monitor its own configuration, disk
space, resources, systemd services, etc.

For instance, if you have Elastic Stack module installed and a log
retention policy set, a service check is added to the above host to
regularly ensure that log retention is actually being carried out.

To perform one of these checks, the installation will also create a
corresponding command ``<cmd>`` along with:

* A service template (``neteye-check-template``) if not already
  created
* A service template called ``<cmd>-neteyelocal-template`` which
  imports ``neteye-check-template``
* A service called ``<cmd>-neteyelocal`` which imports
  ``neteye-check-template``

You should not modify the name of these objects, because the next time
you run ``neteye install`` they will be recreated. Should you not
want one of these NetEye checks to be performed, please disable the
corresponding service.

.. _neteye-health-check:

The NetEye Health Check
```````````````````````

Just as NetEye monitors the states of hosts and servers, it is also
important to monitor the health of NetEye itself. In fact, there are
multiple reasons for monitoring NetEye's health, and thus the health
checks are divided into two types:

- The **Light** check is a sequence of very lightweight checks (think
  **ping**) that tells you quickly whether important parts of NetEye
  are up and running. Running a light check is almost instantaneous,
  with very little computational impact on the NetEye server.

- **Deep** checks instead are intended for tasks like verifying the
  integrity and consistency of resources. They can be computationally
  expensive and are typically used before an :ref:`update or upgrade
  <update-overview>`.

Because the light check has such a slight impact, it can itself be used
as a monitoring command, just like any other monitoring check. This
includes using it in Director (as *neteye check*), where it might run
every 5 minutes, and trigger notifications on a specific result. The
deep check on the other hand should not be used as a monitoring command
because it would interrupt other NetEye functions.

Scenarios in which the use of deep checks is suggested include:

1. Before and after a NetEye upgrade. The update and upgrade
   procedures are indeed important tasks to carry out to ensure that
   NetEye works smoothly and is constantly up to date with the latest
   features and bug fixes. It is therefore a good practice to ensure
   that the health of a NetEye installation, either single or within a
   cluster, is acceptable, both before and after the upgrade. How to
   check the health of NetEye in this scenario is explained in the
   :ref:`dedicated section <neteye-healtcheck-pre-checks>` at the
   bottom of this page.

2. After a power outage. When power goes suddenly out, a number of
   processes may be interrupted while they are operating, with the
   result that data might be lost and configurations might be broken,
   therefore running a deep health check after resuming operation is a
   good practice.

3. When manually troubleshooting a failing light check to find the root
   cause. Light checks might fail for a number of reason, not always
   obvious at a first inspection. To find the real reason for a
   lightweight check to fail, a deep health check might provide a more
   thorough analysis and detailed information about the failure.

Health Checks provided by |ne| Core or Additional Components are performed
with a command that is automatically attached via the *health-check-neteyelocal*
service to the neteye-local host.

Technical Details
+++++++++++++++++

Both the **light** and **deep** checks are implemented as shell commands
that call a set of scripts in a particular order. These scripts can also
be run individually.

.. note:: For security reasons, some of the scripts are not cluster
   aware: they in fact verify the state of the node they are running
   on without checking the states of the other nodes in a NetEye
   cluster. To ensure the cluster is healthy, execute the checks on
   each node and make sure no one is failing.

The syntax of the commands are as follows::

   # neteye health light
   # neteye health deep
   # neteye check

They each return a standard monitoring exit code (a number 0-3). Note
however that only the third command should be used for monitoring
purposes. Also, the current implementation of the *neteye check* command
is to call the light health check.

The scripts called by the health checks can be found in the following
directories::

   /usr/share/neteye/scripts/neteye/health.d/light/
   /usr/share/neteye/scripts/neteye/health.d/deep/

If an error occurs when one of the scripts is running, the output will
also contain the path to the individual script that failed.

.. _neteye-healtcheck-pre-checks:

Pre-Update and Pre-Upgrade Checks
+++++++++++++++++++++++++++++++++

Before any update or upgrade procedure, it is recommended to monitor the
health of the NetEye installation via health checks.

A deep check can be run by typing the following command::

   # neteye health deep
   OK - All deep health checks succeeded.
   [..]

Remember that, in case of a cluster, the checks must be executed on
every node before continuing with the update or the upgrade procedure.

Furthermore, to ensure that an update or upgrade will go smoothly, the
:ref:`neteye update <neteye-update>` or :ref:`neteye upgrade <neteye-upgrade>`
will also check that the system is prepared. For instance, one check will search
for any .rpmnew and .rpmsave files created during the update/upgrade
process.

If one or more of these checks fail, the :command:`neteye update` or
:command:`neteye upgrade` script will halt before the update/upgrade begins. In
this case, it is enough to fix the problem and then re-run the
:command:`neteye update` or :command:`neteye upgrade`.
