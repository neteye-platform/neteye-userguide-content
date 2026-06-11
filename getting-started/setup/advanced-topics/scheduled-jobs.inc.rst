
In NetEye, recurrent  jobs which need to be executed repeatedly at
specific times can be scheduled using `Systemd
Timers <https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html>`__.

Similar to a cronjob, each Timer has a `calendar
event <https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html#Calendar%20Events>`__
defining the points in time when it will be triggered. Each Timer also
has a related Systemd Service Unit which will be started
(``systemctl start $UNIT``) whenever the timer reaches its next
execution time.

The list of active Timers for the current node can be shown using
``systemctl list-timers`` giving you an overview over active timers on
this node::

   [root@neteye02 ~]# systemctl list-timers

.. container:: codeblock

   .. code::

      NEXT                         LEFT     LAST                         PASSED   UNIT           ACTIVATES
      Wed 2020-02-19 09:23:00 CET  46s left Wed 2020-02-19 09:22:00 CET  12s ago  myTimer.timer  myService.service
      [...]


.. note:: In a cluster environment you will need to execute the
   command on each node, to get an overview over all timers of NetEye,
   as some of them are bound to specific services. Additionally there
   can be local timers, with one active copy on each cluster node.
