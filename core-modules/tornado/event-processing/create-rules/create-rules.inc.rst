
.. _adding-rules:

Create a Rule
~~~~~~~~~~~~~

In the case a Tornado Event matches a Rule based on its conditions,
the Rule would trigger an Action, e.g. command, process or operation,
depending on your needs.

1. Select or add a Ruleset in a Processing Tree

   All the rules presented in a Processing Tree belong to a particular Ruleset.
   Thus, you will have to make sure you have selected the right child node,
   i.e. a Ruleset, based on the procedure defined in :ref:`adding-nodes` section.

2. By clicking on a Rule item or on the "Add rule" button, a form to
   add or edit a rule, respectively, opens. It is organized in tabs:

.. _fig-tc-ruleeditor:

.. figure:: ./../img/carbon/rule_editor.png

3. Provide data for the basic **Properties** of a Rule:

-  ``rule name``: A string value representing a unique rule identifier.
   It can be composed only of alphabetical characters, numbers and the
   "_" (underscore) character.
-  ``description``
-  ``continue``: A boolean value indicating whether to proceed with the
   event matching process if the current rule matches.
-  ``active``: A boolean value; if ``false``, the rule is ignored.

4. Define conditions of a rule that would serve as matching principles
   for the events.

   The conditions are defined with the help of Constraints. There are two types of them:

-  **WHERE**: A set of :ref:`operators <where-conditions>` that allows you to specify the condition
   where the rule should be matched; when applied to an event returns ``true`` or ``false``
-  **WITH**: A set of regular :ref:`expressions <tornado-with-clause>` that extract values from an
   Event and associate them with named variables

   An event matches a rule only if the WHERE clause evaluates to ``true`` and
   all regular expressions in the WITH clause return non-empty values.

   Depending on the constrains used, you can add a rule to extract variables
   from the payload of an event. For this, an *extractor* should be added inside
   the WITH tab of a particular rule. All extracted variables are displayed in
   the Test Event panel.

.. _fig-tc-variables:

.. figure:: ./../img/carbon/extracted_variables.png

   Sample of extracted variables
