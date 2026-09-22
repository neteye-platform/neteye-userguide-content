Typical Daily Interaction
~~~~~~~~~~~~~~~~~~~~~~~~~

After the |nec| Support staff has completed your system configuration and
monitoring has begun, you will most likely use |nec| when:

* You need to make a configuration change, like adding or removing hosts,
  services or contacts, in which case you'll use the Support Portal
* You receive a notification or alert, so you'll want to quickly find out
  which host or service is down and begin remediation
* You want to proactively find problems before they happen by studying
  trends in Operations Analytics

|nec| provides you with a number of tools to make these latter two tasks
both more efficient and more effective.

Spotting Problems with the Problem View
```````````````````````````````````````

The **Problems View** collects in a single place all problems of a particular
type, to help you solve multiple cases with a triage approach.  Within each
view, each host and service shown links to its Details panel.

* Host Problems: This view shows a list of all hosts in the Down state. Each
  row shows the host's name, state, time in down state, and the check result
  that lead to that state:

  .. _figure-nec-problems-view-host:

  .. figure:: /neteye-cloud/monitoring/img/problems_view_host.png
     :alt: Screenshot of the host problems view

     The Host Problems view

  The hosts are sorted by severity by default, but can also be sorted by
  current and last state change.

* Service Problems:  This view is similar to the Host Problems view, but
  shows services and their more detailed states and check results. In addition
  to the sorting options for hosts, services can also be sorted by the host
  they run on.
* Service Grid: This view displays a matrix representation with hosts on one
  side and services on the other. By default, it lists all hosts with at least
  one impacted service, and all services on a host with at least one impacted
  service. The boxes in the grid are color coded for state, everything is
  linkable. The grid can be rotated using the box at the top left.
* Current Downtimes: Lists all hosts that are currently under preventative
  maintenance. Shows the amount of time remaining before the specified
  restoration date for the host, and who scheduled the downtime.

  .. _figure-nec-problems-view-downtimes:

  .. figure:: /neteye-cloud/monitoring/img/problems_view_downtimes.png
     :alt: Screenshot of the downtime problems view

     The Downtime Problems view

For each view type you can
`adjust the filters <https://neteye.guide/current/core-modules/director/monitoring-status.html#using-the-custom-problem-view>`_
to select a subset of the monitored objects to be displayed.



Adding Comments to a Monitored Object
`````````````````````````````````````

As you work with hosts and services, you may want to annotate them with short
comments, which will remain visible every time you view the host or service
again, and which you can delete later when they're no longer relevant.

Some types of comments you may find helpful are:

* Reminders to yourself to do something the next time you see this monitored object
* Creating a record when a problem repeats, to help you diagnose it later
* Messages to fellow sysadmins about plans for specific hosts or services
* Notes describing thoughts hard to capture in numbers, like relative importance

You can add comments directly from a monitored object view as shown here:

.. _figure-nec-comment-add-via-gui:

.. figure:: /neteye-cloud/monitoring/img/comment-add-via-gui.png
   :alt: Adding a comment directly to a monitored object

   Adding a comment directly to a monitored object

.. note:: The |nec| Support team can also add or delete comments in bulk for you
   using filterable conditions with the Icinga 2
   `add-comment <https://icinga.com/docs/icinga-2/latest/doc/08-advanced-topics/#comments>`_
   API action.

Once added, a comment can be viewed by going to the details panel for that
host. To see all comments and be able to filter them according to your own
criteria, go to dedicated view at **Overview > Comments**. Comments remain
until deleted.


Acknowledging an Alert
``````````````````````

When you've received a notification or alert, you may want to quickly let
others know that you're aware of the issue and are working on a fix. You can
explicitly do this for a monitored object
`with an acknowledgement <https://icinga.com/docs/icinga-2/latest/doc/08-advanced-topics/#acknowledgements>`_,
which will send a notification message to other users or admins.

By default an acknowledgement will be removed if the host/service recovers
(OK/Up) or a state change occurs. To keep an acknowledgement there until
an issue moves from partially recovered to completely recovered, you can use
the *sticky* parameter.

Like comments, the |nec| Support team can also add or delete comments in bulk
for you using filterable conditions with the *Icinga 2 acknowledge-problem*
command.


Investigating Past Monitoring Events
````````````````````````````````````

If you haven't arrived at NetEye because of an alert or notification (for
instance you do a daily morning check) the first step is almost always to
look at the Dashboard to see if anything important is going on.

But suppose you've just come back from a day or two off and you want to see
what's happened in that time.  The *History* section contains views that
show you what important events have occurred, with a customizable filter:

* Event Overview: All the state transitions of monitored objects, showing
  the new state type, when it occurred, and the object affected

  .. _figure-nec-history-event-overview:

  .. figure:: /neteye-cloud/monitoring/img/history_event_overview.png
     :alt: Viewing the Event Overview in the History module

     Viewing the Event Overview in History

* Notifications: All the notifications and alerts that were sent in a
  given time range, beginning with the most recent

  .. _figure-nec-history-notifications:

  .. figure:: /neteye-cloud/monitoring/img/history_notifications.png
     :alt: Viewing the Notification log in the History module

     Viewing the Notification log in History


Investigating More Deeply
`````````````````````````

When NetEye receives the results of performance-based monitoring checks,
it stores them in a database in order to display that historical data for
your hosts and services.

Over extended periods of time, or for the results of very frequent checks,
it's not enough to see just the raw data in text form that's then displayed
by Icinga 2. Similarly, the graphs displayed in the Host and Service Details
panels is in graph form, which is very helpful to quickly see simple patterns
in recent data.

IT Operations Analytics on the other hand is like a set of supercharged
graphs: it allows for interactivity in multiple ways, such as in setting the
time range of what's visible, choosing a subset of data streams that are
visible at any given moment, and computing functions involving one or more
data streams.

NetEye uses `Grafana <https://grafana.com/>`_ to store and display
interactive graphs using time-series data, with highly customizable features
that can be either added from pre-existing dashboard templates, or that you
yourself can create using Grafana's built-in graphing language.

  .. _figure-nec-itoa-monitoring-example:

  .. figure:: /neteye-cloud/monitoring/img/itoa-monitoring-example.png
     :alt: Screenshot of an interactive ITOA graph on time-series data

     An interactive ITOA graph showing time-series data
