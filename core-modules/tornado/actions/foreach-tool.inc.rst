.. _tornado-foreach-executor:

Foreach Tool
~~~~~~~~~~~~

The Foreach Tool loops through a set of data and executes a list of
actions for each entry; it extracts all values from an array of
elements and injects each value to a list of action under the *item*
key.

There are two **mandatory** configuration entries in its payload:

-  **target**: the array of elements, e.g. ``${event.payload.list_of_objects}``
-  **actions**: the array of action to execute

In order to access the item of the current cycle in the actions inside the Foreach Tool,
use the variable ``${item}``.
