.. _tornado-script-executor:

Script
~~~~~~

SCRIPT Action type allows to run custom shell scripts on a Unix-like system
in order to customize the Action according to your needs.

In order to be correctly processed by the Script Executor, an Action should provide two
entries in its payload: the path to a script on the local filesystem
of the Executor process, and all the arguments to be passed to the
script itself.

The script path is identified by the payload key **script**.

.. code:: bash

   neteye# ./usr/share/scripts/my_script.sh

It is important to verify that the Executor has both read and execute rights
at that path.

The script arguments are identified by the payload key **args**; if
present, they are passed as command line arguments when the script is
executed.
