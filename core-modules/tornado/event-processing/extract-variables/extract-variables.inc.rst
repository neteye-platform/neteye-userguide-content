.. _extract-variables:

Extract Variables
~~~~~~~~~~~~~~~~~

In order to be able to define the action in a dedicated rule, you might first need to
extract or modify data from the payload of the event.

This can be achieved by creating a dedicated Extractor in a Rule, where a single
or multiple variables are extracted with the help of conditions in the *WITH* clause.
The *WITH* clause generates variables extracted from the Event based on
regular expressions. These variables can then be used to populate an
Action payload.

Three simple rules restrict the access and use of the extracted variables:

1. Extracted variables are evaluated after the *WHERE* clause is parsed. For this reason
   any extracted variables declared inside the *WITH* clause are not
   accessible by the *WHERE* clause of the same rule
2. The order in which variables are extracted within a WITH clause of a rule is not guaranteed.
   For this reason, it is not recommended to access a variable extracted in the same WITH clause
3. A rule can use extracted variables declared by previous rules of the ruleset, even in
   its *WHERE* clause, provided that:

   -  The two rules must belong to the same rule set
   -  The rule attempting to use those variables should be executed
      after the one that declares them
   -  The rule that declares the variables must also match the event

.. note::

   All variables declared by a Rule must be resolved, or else the Rule will not be matched.


The syntax for accessing an extracted variable has the form:

.. code:: bash

   _variables.[.<RULE_NAME>].<VARIABLE_NAME>

If the *RULE_NAME* is omitted, the search of a variable is performed in the current rule.

You may need to extract the desired value from an incoming event in many cases, for example
setting a particular status to a host object based on the device battery level reported
in the event.

Assuming an SMS Event is received,

.. code:: json

   {
     "event_type": "sms",
     "created_ms": 1695977917962,
     "payload": {
       "sender": "+393333333333",
       "modem": "GSM1"
       "timestamp": 1695922157429,
       "text": "Device battery level: 56%"
     }
   }

an extractor Rule with the regex in the WITH clause is to be created in order to define
the battery level value

.. figure:: ./../img/carbon/with_config.png

As result, the output after extracting the variable will be available in the UI:

.. figure:: ./../img/carbon/variable_output.png

The extracted variable will serve as a basis for defining the action in another Rule of the same Ruleset,
e.g. setting "Critical" status to the host object in case the value is lower than 20%.

For this the extracted variable is to be modified with the help of the Modifiers,
e.g. map the extracted value to the status that will be displayed in the NetEye Dashboard.

For more cases of WITH Modifiers' usage please consult  :ref:`post-modifiers`.
