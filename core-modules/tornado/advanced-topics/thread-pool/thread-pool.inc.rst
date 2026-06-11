Thread Pool Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~

Even if the default configuration should suit most of the use cases, in
some particular situations it could be useful to customise the size of
the internal queues used by Tornado. Tornado utilizes these queues to
process incoming events and to dispatch triggered actions.

Tornado uses a dedicated thread pool per queue; the size of each queue
is by default equal to the number of available logical CPUs.
Consequently, in case of an action of type *script*, for example,
Tornado will be able to run in parallel at max as many scripts as the
number of CPUs.

This default behaviour can be overridden by providing a custom
configuration for the thread pools size. This is achieved through the
optional **tornado_pool_config** entry in the **tornado.daemon** section
of the *Tornado.toml* configuration file.

.. rubric:: Example of Thread Pool's Dynamical Configuration

.. code:: toml

   [tornado.daemon]
   thread_pool_config = {type = "CPU", factor = 1.0}

In this case, the size of the thread pool will be equal to
``(number of available logical CPUs) multiplied by (factor)`` rounded to
the smallest integer greater than or equal to a number. If the resulting
value is less than *1*, then *1* will be used be default.

For example, if there are 16 available CPUs, then:

-  ``{type: "CPU", factor: 0.5}`` => thread pool size is 8
-  ``{type: "CPU", factor: 2.0}`` => thread pool size is 32

.. rubric:: Example of Thread Pool's Static Configuration

.. code:: toml

   [tornado.daemon]
   thread_pool_config = {type = "Fixed", size = 20}

In this case, the size of the thread pool is statically fixed at 20. If
the provided size is less than *1*, then *1* will be used be default.
