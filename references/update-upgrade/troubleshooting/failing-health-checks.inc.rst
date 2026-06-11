
...during the update/upgrade procedure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The NetEye update or upgrade commands run all the deep health checks
to ensure that the NetEye installation is healthy before running the update or
upgrade procedure. It might happen, however, that one of the check fail, thus
preventing the procedures to complete successfully.

Hence, to manually solve the problem you should follow the directions
that can be found in section :ref:`The NetEye Health Check <neteye-health-check>`.

Once the issue is solved, the NetEye update/upgrade commands can be run again.


...after the finalization procedure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After the finalization procedure has successfully ended, you might
notice in the Problems View (see :menuselection:`Menu / Problems`) that
some :ref:`health check <neteye-health-check>` fails and is in state **WARNING**.
The reason is that you are using some module that *needs to be migrated*, because
some breaking change has been introduced in the release.

Hence, you should go to the Problems View and check which health check
is failing. There you will also find instructions for the correct
migration of the module, which is in almost all cases amounts to
enabling an option: the actual migration will then be executed
manually.
