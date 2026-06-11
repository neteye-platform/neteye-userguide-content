All the NetEye commands that you will be running to update or upgrade,
such as :command:`neteye update` and :command:`neteye upgrade`,
store their output in :file:`/neteye/local/os/log`.
These logs are useful in case you need to troubleshoot any issues
that may arise during the update or upgrade process.

The :command:`update` and :command:`upgrade` commands can take a long time to execute,
and it is important to ensure that they are not interrupted by a dropped connection or a timeout.
If the update or upgrade process is interrupted, it may leave your system in an inconsistent state,
which can cause problems and require additional troubleshooting to fix.

To avoid this, every command that may take a long time,
is presented with the :command:`nohup` command at the beginning,
which allows the command to run in a detached bash session.

In the following example, the :command:`neteye update` command is run in the background with all output written to **nohup.out** file:

.. code:: bash

    # (nohup neteye update &) && tail --retry -f nohup.out

In this way, even if the connection drops, the command will continue to run and its output will be saved to :file:`nohup.out` file,
allowing you to check its progress and results as soon as the connection is reinstated.

In case the connection drops, you can simply log back into the system and check the progress of the command by examining the :file:`nohup.out` file.

.. code:: bash

   # tail --retry -f nohup.out

For more information, please refer to the official :manpage:`nohup(1)`
documentation.
