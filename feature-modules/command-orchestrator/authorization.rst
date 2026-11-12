
.. _command-orchestrator-authorization:

Authorization in the Command Orchestrator Module
------------------------------------------------

The |ne| administrator can create Command Groups and assign them to
:ref:`Roles <auth-roles>`.  If a role is configured with the execute
permission, users with that role can execute the commands in those
groups.

A user having access to a Command Group has access to all the commands
in that group, and to all its descendants.

.. note:: The execute permission is module-wide; this implies that the user
   can execute commands in all commands groups he or she has access to. When
   multiple roles are associated with a user, command execution is enabled
   if this permission is enabled in at least one of the roles.

Each |ne| user can only execute commands on the hosts which he or she has
access to in the Icinga DB module. In addition to this restriction,
each command defines which hosts the command itself can be executed on.

|
