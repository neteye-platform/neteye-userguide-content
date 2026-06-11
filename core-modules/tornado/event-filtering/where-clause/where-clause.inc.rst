.. _where-conditions:

WHERE Conditions
~~~~~~~~~~~~~~~~

While configuring your Filter node, a number of conditions can be applied
in order to match the incoming Events of a certain type, origin, etc.
These conditions are to be defined as WHERE operators in a dedicated tab
of the Tornado Filter configuration form.

If the *WHERE* clause is not specified, the filter passes all events' matching process
to its children nodes.


.. figure:: ./../img/where-conditions/where-tab.png

Based on your needs, a single or multiple operators can be applied for filtering particular events.
Below you will find all available WHERE operator options and their details.

.. _tornado-andornot-operator:

'AND', 'OR', and 'NOT'
``````````````````````

As a first step, select a group of conditions that are to be applied, which
consist of 'AND', 'OR', and 'NOT' options.

The 'AND' and 'OR' groups work on a set of operators, that are to be further selected,
while the 'NOT' group works on a single operator.

- The 'AND' condition evaluates to `true` if all inner operators match:

  .. figure:: ./../img/where-conditions/group-and.png

  The filter matches only the events of type SNMP **and** that are sent from host1.


- The 'OR' condition evaluates to true if at least one of the inner operators matches:

  .. figure:: ./../img/where-conditions/group-or.png

  The filter matches only events of type SMS **or** email.


- The 'NOT' group evaluates to `true` if the inner operator does not
  match, and evaluates to `false` if the inner operator matches:

  .. figure:: ./../img/where-conditions/group-not.png

  The filter matches every event that is **not** coming from a host that contains ``.localdomain`` in its name.


The groups can be nested recursively to define complex conditions for a filter to recognize certain events:

.. figure:: ./../img/where-conditions/and_or_not_combined.png

The filter matches events of type SMS **or** email **and** from hosts that do **not** contain ``.localdomain`` string in their names.



.. _tornado-contains-operator:

'CONTAINS'
``````````

The operator evaluates whether the first argument contains the second one.
It can be applied to strings, arrays, and maps.

It applies in three different situations:

-  The arguments are both strings: Returns true if the second string is
   a substring of the first one.
-  The first argument is an array: Returns true if the second argument
   is a part of an array.
-  The first argument is a map and the second is a string: Returns true
   if the second argument is an existing key in the map.

.. figure:: ./../img/where-conditions/contains.png

In any other case, it will return false.

.. _tornado-containsignorecase-operator:

.. rubric:: The 'CONTAINSIGNORECASE'

The 'CONTAINSIGNORECASE' operator is used to check whether the first
argument contains the **string** passed as second argument, regardless
of their capital and small letters. In other words, the arguments are
compared in a *case-insensitive* way.

It applies in three different situations:

-  The arguments are both strings: Returns true if the second string is
   a *case-insensitive substring* of the first one
-  The first argument is an array: Returns true if the array passed as
   first parameter contains a (string) element which is equal to the
   string passed as second argument, regardless of uppercase and
   lowercase letters
-  The first argument is a map: Returns true if the second argument
   contains, an existing, *case-insensitive*, key of the map

In any other case, this operator will return false.

.. figure:: ./../img/where-conditions/contains-ignore.png


A Filter will match events if their payload contains an entry with key
"hostname" and whose value is a string that contains "linux", **ignoring
the case** of the strings, e.g.

.. code:: json

   {
       "type": "trap",
       "created_ms": 1554130814854,
       "payload":{
           "hostname": "LINUX-server-01"
       }
   }

Additional values for *hostname* can be:
**linuX-SERVER-02**, **LInux-Host-12**, **Old-LiNuX-FileServer**, and so
on.

.. _tornado-comparison-operators:

Comparison:'EQUALS', 'GE', 'GT', 'LE', 'LT' and 'NE'
````````````````````````````````````````````````````

These operators are used to compare two values to find if they are equal,
different, or one is greater/smaller than the other:

-  **'EQUALS'**: Compares any two values (including, but not limited to,
   arrays, maps) and returns whether or not they are equal.
-  **'GE'**: Compares two values and returns whether the first value is
   greater than or equal to the second one. If one or both of the values
   do not exist, it returns ``false``.
-  **'GT'**: Compares two values and returns whether the first value is
   greater than the second one. If one or both of the values do not
   exist, it returns ``false``.
-  **'LE'**: Compares two values and returns whether the first value is
   less than or equal to the second one. If one or both of the values do
   not exist, it returns ``false``.
-  **'LT'**: Compares two values and returns whether the first value is
   less than the second one. If one or both of the values do not exist,
   it returns ``false``.
-  **'NE'**: This is the negation of the **'equals'** operator. Compares
   two values and returns whether or not they are different.

All these operators can work with values of type Number, String, Bool,
null and Array.

.. warning:: Please be extremely careful when using these operators
   with numbers of type **float**. The representation of floating
   point numbers is often slightly imprecise and can lead to
   unexpected results (for example, see
   https://www.floating-point-gui.de/errors/comparison/ ).


.. figure:: ./../img/where-conditions/equals.png


Example 2:

.. figure:: ./../img/where-conditions/comparison.png


The filter will match events if *event.payload.value* exists and one or
more of the following conditions are held:

-  It is equal to *1000*
-  It is between *100* (inclusive) and *200* (inclusive), but not equal
   to *150*
-  It is less than *0* (exclusive)
-  It is greater than *2000* (exclusive)

A matching Event is:

.. code:: json

   {
       "type": "email",
       "created_ms": 1554130814854,
       "payload":{
         "value": 110
       }
   }

Here are some examples showing how these operators behave:

-  ``[{"id":557}, {"one":"two"}]`` *lt* ``3``: *false* (cannot compare
   different types, e.g. here the first is an array and the second is a
   number)
-  ``{id: "one"}`` *lt* ``{id: "two"}``: *false* (maps cannot be
   compared)
-  ``[["id",557], ["one"]]`` *gt* ``[["id",555], ["two"]]``: *true*
   (elements in the array are compared recursively from left to right:
   so here "id" is first compared to "id", then 557 to 555, returning
   true before attempting to match "one" and "two")
-  ``[["id",557]]`` *gt* ``[["id",555], ["two"]]``: *true* (elements are
   compared even if the length of the arrays is not the same)
-  ``true`` *gt* ``false``: *true* (the value 'true' is evaluated as 1,
   and the value 'false' as 0; consequently, the expression is
   equivalent to "1 gt 0" which is true)
-  "twelve" *gt* "two": *false* (strings are compared lexically, and 'e'
   comes before 'o', not after it)

.. _tornado-equalsignorecase-operator:

.. rubric:: The 'EQUALSIGNORECASE'

The `EQUALSIGNORECASE` operator is used to check whether the strings
passed as arguments are equal in a *case-insensitive* way.

It applies **only if** both the first and the second arguments are
strings. In any other case, the operator will return false.

.. figure:: ./../img/where-conditions/equals-ignore.png

A filter will match the event with payload that has an entry with key
"hostname" and whose value is a string that is equal to "linux",
**ignoring the case** of the strings.

A matching Event is:

.. code:: json

   {
       "type": "trap",
       "created_ms": 1554130814854,
       "payload":{
           "hostname": "LINUX"
       }
   }

.. _tornado-regex-operator:

'REGEX'
```````

The 'REGEX' operator is used to evaluate if a field of an event matches a given
regular expression. The evaluation is performed with the Rust Regex library (see
its `github project home page <https://github.com/rust-lang/regex>`__ )

.. note:: We use the Rust Regex library (see its `github project home
   page <https://github.com/rust-lang/regex>`__ ) to evaluate regular
   expressions provided by the *WITH* clause and by the *regex* operator.
   Refer to its `dedicated documentation <https://docs.rs/regex>`__ for details about its features
   and limitations.
   You can also visit `this site <https://regex101.com>`__ in order to test your regex
   input and get interactive feedback on its syntax.

For example, when applying the following value for a REGEX operator,


.. figure:: ./../img/where-conditions/regex.png

the Filter will match events where the type matches regular expression
[a-fA-F0-9].

A matching Event is:

.. code:: json

   {
       "type": "trap0",
       "created_ms": 1554130814854,
       "payload":{}
   }
