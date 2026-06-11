
Authorization in the Command Orchestrator Module
------------------------------------------------

The NetEye administrator can create Command Groups and assign them to
:ref:`Roles <auth-roles>`.  If the Role is configured with execute
permission, Users with that Role can execute the Commands in the
Groups.

A user having access to a Command Group has access to all the Commands
in that Group, and to all its descendants.

.. note:: The execute permission is module-wide; this implies that the User
   can execute commands in all Commands Groups he has access to. In
   case multiple roles are associated to a user, command execution is
   enabled if in at least one of the roles this permission is enabled.

Each NetEye user can only execute Commands on the hosts which he has
access to in the Icinga DB module. In addition to this restriction,
each Command defines on which hosts the Command itself can be executed.
