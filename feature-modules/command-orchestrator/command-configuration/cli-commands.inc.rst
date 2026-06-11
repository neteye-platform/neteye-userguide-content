Command Orchestrator Configuration CLI Commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can configure the Command Orchestrator directly from the shell with
the *icingacli* command.

By using the Command Orchestrator's CLI commands, you can perform
`create, edit, delete and list` actions on the following
*cmdorchestrator* objects:

#. **Command:** A Command which contains all the information for the
   execution of a command.

#. **Command Group:** A group representing a set of Commands. A
   Command Group can be *child* of another Command Group. Each Command
   Group can then contain some other Command Groups and some Commands.
#. **Command Parameter:** A Command Parameter which contains all the
   information for the possible value of the parameter (macro) i.e.,
   `$param$`.

   .. note:: Once you finish configuring the Command Orchestrator
      objects, **you need to deploy the Director configuration** in
      order to execute the Commands.

Below you can find detailed descriptions of the available commands and
their parameters.

Command Orchestrator User
`````````````````````````

Command orchestrator use a specific user called **cmdorchestrator** to
access icinga2 APIs. The configuration is automatically generated during
:command:`neteye install` with a random password and must not be modified
by the user.

Credentials can be found in the file
:file:`/neteye/shared/icinga2/conf/icinga2/conf.d/cmdorchestrator-users.conf`
e.g.::

    object ApiUser "cmdorchestrator" {
      password = "sBNL...vViO"
      permissions = [ "objects/query/host","objects/query/service","actions/execute-command" ]
    }

**cmdorchestrator** user has only permissions to query host and services
and execute commands.

CLI configuration of Command
````````````````````````````

.. _cmdo-cli-command-create:

.. rubric:: Create


The *create* command lets you construct a new Command. It requires a
name, a type for the command, the command to be executed, the parameters
with which the command must be executed, a monitoring object filter
(only in case of ``remote`` command) and a *Command Group* ID.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator command create [parameters]

**Available Parameters:**

:*-\-name*: (mandatory) The name of the Command to be created.

:*-\-command-type*: (mandatory) The type of the Command, which can take one of
    the following values:

    * ``remote`` : the Command will be executed remotely on the host/s
        specified upon execution.

    * ``local`` : the Command will be executed from the NetEye host.

    * ``weblink`` : the execution of the Command will open a link in a new tab
        of the user's browser. The link will point to the url specified in the
        ``command`` field

:*-\-command*: (mandatory) The command (the executable) to be executed.

:*-\-command-parameters*: (optional) A JSON array defining the parameters with
    which ``command`` will be executed. Parameters can include **macros** in
    the form ``$myparam$``, which will be substituted upon execution.

:*-\-monitoring-object-filter*: (mandatory only if ``command-type`` is
    ``remote`` ) A Monitoring Object Filter which restricts the execution of
    the command to the set of hosts defined by the filter.

:*-\-command-group-id*: (mandatory) The ID of the Command Group to which this
    Command belongs to.

:*-\-description*: (optional) A text description of what the Command
    Parameter represents.

:*-\-timeout*: (optional) The maximum time the command is allowed to run before
    it is killed. The value should be a number, followed by a unit (s)econds,
    (m)inutes, (h)ours or (d)ays, e.g. 60s, 1.5m, 0.25h, etc. The timeout
    defaults to the standard icinga2 timeout duration of 60 seconds.


**Example:**

.. code:: bash

    # icingacli cmdorchestrator command create \
                --name 'touch_file' \
                --command-type 'remote' \
                --command '/usr/bin/touch' \
                --command-parameters '["/tmp/myfile"]' \
                --monitoring-object-filter 'host.name=neteye*' \
                --command-group-id '1' \
                --description 'This command will create the selected file on the system'
                --timeout '1s'

.. rubric:: Edit


By using the *edit* command, you change one or more of the values for
the fields in an existing Command using the :ref:`same parameters
<cmdo-cli-command-create>` as the ``command create`` command above.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator command edit [parameters]

.. rubric:: List


With *list* command you can see a list of all existing Commands in JSON
format.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator command list

.. rubric:: Delete


The *delete* command allows you to remove an existing Command given its
ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator command delete [parameters]

**Available Parameters:**

:*-\-id*: (mandatory) The ID of the Command to delete

CLI configuration of Command Group
``````````````````````````````````

.. rubric:: Create


You can use the *create* command to construct a new Command Group. It
requires a name and, optionally, a description and a parent Command
Group ID.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandgroup create [parameters]

**Available Parameters:**

:*-\-name*: (mandatory) The name of the Command Group to be
       created. All alphanumeric characters are allowed in the
       name, plus the characters ``-`` and ``_``.

:*-\-description*: (optional) A text description of what the Command Group
    represents.

:*-\-parent-command-group-id*: (optional) The ID of the Command Group which is
    the *parent* of this Command Group. If not specified, the newly created
    Command Group will have no *parent* and will then be at the top of the
    hierarchy.

**Example:**

.. code:: bash

    # icingacli cmdorchestrator commandgroup create \
                --name 'linux' \
                --description 'commands to be run on linux systems' \
                --parent-command-group-id '1'

.. rubric:: Edit


With the *edit* command it is possible to change one or more of the
values for the fields in an existing Command Group using the
:ref:`same parameters <cmdo-cli-command-create>` as the ``commandgroup
create`` command above.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandgroup edit [parameters]

.. rubric:: List


With the *list* command you can see a list of all existing Command
Groups in JSON format.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandgroup list

.. rubric:: Delete


The *delete* command allows you to remove an existing Command Group
given its ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandgroup delete [parameters]

**Available Parameters:**

:*-\-id*: (mandatory) The ID of the Command Group to delete

CLI configuration of Command Parameter
``````````````````````````````````````

.. rubric:: Create


By using the *create* command you construct a new Command Parameter
i.e., Macro. It requires a command ID, a parameter name (macro), a type
for the parameter and, optionally, a possible values for the command
parameter.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandparameter create [parameters]

**Available Parameters:**

:*-\-command-id*: (mandatory) The ID of the Command to which this
    macro (a dynamic Command parameter) belongs to.

:*-\-parameter*: (mandatory) The name of the macro for the Command.

:*-\-parameter-type*: (mandatory) The type of the macro i.e, string or
    number.

:*-\-description*: (optional) A text description of what the Command
    Parameter represents.

:*-\-label*: (optional) A text that is shown instead of the parameters
    name in the UI.

:*-\-possible-values*: (optional) The possible values that a macro can
              take when a user executes a Command. The
              possible value is only required with
              *parameter-type* as string and must be valid
              JSON array which contains only string values.

**Example:**

.. code:: bash

    # icingacli cmdorchestrator commandparameter create \
                --command-id '1' \
                --parameter '$touch_file$' \
                --parameter-type 'string' \
                --possible-values '["/tmp/testfile1", "/tmp/testfile2"]' \
                --description 'The file on the system you want to touch' \
                --label 'File path'

.. rubric:: Edit


The *edit* command allows to change one or more of the values for the
fields in an existing Command Parameter using the :ref:`same
parameters <cmdo-cli-command-create>` as the ``commandparameter
create`` command above.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandparameter edit [parameters]

.. rubric:: List


With the *list* command you can see a list of all existing Command
Parameter in JSON format.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandparameter list

.. rubric:: Delete


The *delete* command lets you remove an existing Command Parameter given
its ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

    # icingacli cmdorchestrator commandparameter delete [parameters]

**Available Parameters:**

:*-\-id*: (mandatory) The ID of the Command Parameter to delete
