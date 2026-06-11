Overview
--------

Executing predefined commands (also remotely) on hosts without having
access to them can be very useful in many situations. For example, a
first level support agent using NetEye may need to restart a Windows
service on a Windows host without access to it.

The Command Orchestrator module lets the NetEye admin define Commands
and Groups of Commands which can be assigned to other non-admin NetEye
users, who will then be able to execute them through the Command
Orchestrator module itself.

.. note:: The configuration of the Command Orchestrator can be currently
   carried out from the command line **only**.

Architecture of the Command Orchestrator Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Command Orchestrator Manager module is based on three main objects:
the **Command**, the **Command Group**, **Command Parameter**. Each
Command must belong to a Command Group, and Command Groups can be
sub-groups of other Command Groups.

-  A **Command** defines the execution of a command and is composed of:

   -  a **name** for the command
   -  a **command type**, which defines if the Command is to be executed
      remotely, locally on NetEye, or if it is a web link.
   -  a **command**, which is the actual executable to be executed on
      the host
   -  some **command parameters** that define with which parameters the
      Command must be launched. A parameter of the Command can also be a
      *macro*, whose value will be chosen by the user who executes the
      command. The possible values for each *macro* have to be declared
      in the Command Parameter objects.
   -  a **monitoring object filter** that defines on which hosts the
      command can be executed
   -  the **Command Group** which the Command is part of

-  A **Command Group** permits to create groups of Commands, for an
   easier management. It can either be at the top of the hierarchy of
   the Command Groups, or can be a child of another Command Group.

-  A **Command Parameter** defines which are the possible values that a
   *macro* (a dynamic Command parameter) can take when a user executes a
   Command. The administrator defines in the Command Parameter object if
   the *macro* can take either any *numeric* value, or any *string*
   value coming from a predefined list of strings.
