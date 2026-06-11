
.. _soc-detection-rules:

Elastic Rules
~~~~~~~~~~~~~

In this section, we will explore the Rules section of Elastic, which contains the detection rules.

Elastic includes more than 600 detection rules, and on top of that more custom rules have been added by the SOC team.
To access the Rules section, first select any category from the Security menu:

.. figure:: /neteye-cloud/soc-ads/img/security-menu.png
   :align: center
   :figwidth: 50%

   The Security menu initially visible


then select Rules:

.. figure:: /neteye-cloud/soc-ads/img/security-rules.png
   :align: center
   :figwidth: 50%

   Other options in the Security menu once you enter the section

Once logged in, a notice will appear at the top informing the user that their role is read-only
and that they cannot edit the rules. However, it is possible to check their status (succeeded
or failed) and whether they are active.

Clicking on a rule's name provides additional details, such as the query behind it,
how often the rule runs, and more.

.. figure:: /neteye-cloud/soc-ads/img/rules-details.png

These rules constantly monitor the data present in Elastic and generate alerts that can be analyzed as explained in :ref:`Alerts <elastic-alerts>` section.

Email connector for alert notifications
```````````````````````````````````````

For each rule, you can set up a connector that can send an email if suspicious behavior
is detected and consequently generate an alert.

This connector is disabled by default because, depending on the rule, it could generate a large volume of emails.
If you're interested in enabling it for all or just particular rules, inform your contact from the SOC team.
