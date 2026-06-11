Using the Custom Problem View
`````````````````````````````

Access to the Problem View requires the user to have a role with
appropriate permissions in the **Icinga DB** module; a Role with
*General Module Access* suffices for the purpose.

Starting from Release **4.13**, a new functionality has been added to
the Problems View. The new module *customproblemview* indeed, defines a
new permission in the form of a filter called
``customproblemview/excludefilter/objects``, which allows to define a
suitable filter to show only a subset of host problems, service
problems, or both.

In **Configuration > Access Control > Roles**, you can define the
``customproblemview/excludefilter/objects``, that represents the
monitoring filter, selecting the monitoring objects you want to filter
out from the Problem View.

For example, let's suppose that your monitoring system is monitoring
both production and test environments. All hosts belonging to test
environment belong to a "test-system" hostgroup. If you wan to see only
the production hosts in the Problem View, you just need to create a role
with the proper filter in ``customproblemview/excludefilter/objects``
(e.g. ``hostgroup.name=test-system``).

Note that if a user belongs to more than one role with a specified
``customproblemview/excludefilter/objects``, the filter will become more
and more selective, showing only objects that belong to all the filters.
