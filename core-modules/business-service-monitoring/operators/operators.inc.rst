Operators
~~~~~~~~~


Every Business Process requires an Operator. This operator defines its
behaviour and specifies how its very own state is going to be
calculated.

AND
   The ``AND`` operator selects the **WORST** state of its child nodes.

OR
   The ``OR`` operator selects the **BEST** state of its child nodes.

DEGRADED
   The ``DEGRADED`` operator behaves like an ``AND``, but if the resulting
   state is **CRITICAL** it transforms it into a **WARNING**.
   Refer to :numref:`figure-bp-degraded-operator` for the case-by-case
   analysis of the statuses.

.. _figure-bp-degraded-operator:

.. figure:: /core-modules/business-service-monitoring/img/operators/deg-operator.jpg

   All possible status outcomes when applying the DEGRADED operator.

MIN
   The ``MIN`` operator selects the **WORST** state out of the **BEST n**
   child node states.
