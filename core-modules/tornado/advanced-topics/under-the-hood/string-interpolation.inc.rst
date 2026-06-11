.. _string-interpolation:

String Interpolation
++++++++++++++++++++

An action payload can contain text with placeholders that Tornado
will replace at runtime. The values to be used for the substitution are
extracted from the incoming *Events* following the conventions mentioned
in the previous section; for example, using that Event definition, this
string in the action payload::

  Received a ${event.type} with protocol ${event.payload.protocol}

produces::

  *Received a trap with protocol UDP*

.. note:: Only values of type *String*, *Number*, *Boolean* and *null*
   are valid. Consequently, the interpolation will fail, and the
   action will not be executed, if the value associated with the
   placeholder extracted from the Event is an *Array*, a *Map*, or
   undefined.
