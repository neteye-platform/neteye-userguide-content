.. _protect-credentials:

Protecting Credentials in the Monitoring Views
``````````````````````````````````````````````

The monitoring views display the values for all fields configured for a
host or service. These views will also display any custom variables you
define as fields in the view. If one or more fields contain sensitive
information such as MySQL accounts, passwords, or SNMP Community
Strings, best practice is to hide the values of those fields in the
monitoring views (they will remain visible in Director’s configuration
panels).

.. _figure-protecting-credentials:

.. figure:: /core-modules/director/monitoring-status/img/protecting-credentials-01.png
   :alt: protecting credentials

   Specifying fields containing credentials.

To do this, navigate to **Configuration > Access Control** and locate the **Restrictions**
panel for a role (:numref:`figure-protecting-credentials`). Set a
pattern in the `icingadb/​protect/​variables` field to hide values for all fields derived from custom
variables whose names match the pattern. This pattern
goes in the field “Protected Custom Variables”, where the syntax is
`defined by Icinga
<https://icinga.com/docs/icinga-db-web/latest/doc/04-Security/#protections>`_.

To summarize, this field should contain a comma separated list of case
insensitive pattern strings, where :strong:`*` is the only allowed matching
operator. Note that any spaces you include will count as part of the
pattern between commas. The field is initially empty, with the suggested
example `*pw*,*pass*,community*`, which would match the strings
“pw”, “password”, and “Community”, among others.

If a field name matches the pattern, then that field’s value will be
replaced with asterisks as shown in
:numref:`figure-protected-password`, hiding the real contents from
view.

.. _figure-protected-password:

.. figure:: /core-modules/director/monitoring-status/img/protecting-credentials-02.png
   :alt: protected passwords

   Protected password in the host monitoring view.
