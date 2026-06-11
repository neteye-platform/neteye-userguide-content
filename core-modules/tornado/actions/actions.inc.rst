Overview
~~~~~~~~

The important part of creating a Tornado Rule is specifying the Actions
that are to be carried out in the case an Event matches a specific Rule.

A selection of Actions available to be defined and configured is presented
in a Rule configuration under the dedicated 'Actions' tab. Each individual Action type
is taken care of by a particular Tornado Executor, which would trigger the associated
executable instructions.

There are several Action types that may be singled out according to their logic.

- **Monitoring Actions** - are carried out by the Icinga 2 Executor, Smart Monitoring
  Check Result Executor and the Director Executor. These Actions are meant for triggering
  the actual monitoring instructions, e.g. setting process check results or creating hosts.

- **Logging Actions** - serve for logging data, be it an Event received from the Action,
  or the output of an Action. This type of actions are carried out by the Logger Executor,
  Archive Executor and Elasticsearch Executor.

In addition to executing a Monitoring or Logging type of action mentioned above, you can
customize the processing of an Action by running custom scripts with the help of :ref:`Script Executor<tornado-script-executor>`,
or loop through a set of data to execute a list of Actions for each entry with the :ref:`tornado-foreach-executor`.

Below you will find a full list of Action types available to be defined in a Rule's 'Actions' tab.
