Retry Strategy Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tornado allows the configuration of a global *retry strategy* to be
applied when the execution of an Action fails.

A *retry strategy* is composed by:

-  *retry policy*: the policy that defines whether an action execution
   should be retried after an execution failure;
-  *backoff policy*: the policy that defines the sleep time between
   retries.

Valid values for the *retry policy* are:

-  ``{type = "MaxRetries", retries = 5}`` => A predefined maximum amount
   of retry attempts. This is the default value with a retries set to
   20.
-  ``{type = "None"}`` => No retries are performed.
-  ``{type = "Infinite"}`` => The operation will be retried an infinite
   number of times. This setting must be used with extreme caution as it
   could fill the entire memory buffer preventing Tornado from
   processing incoming events.

Valid values for the *backoff policy* are:

-  ``{type = "Exponential", ms = 1000, multiplier = 2 }``: It increases
   the back off period for each retry attempt in a given set using the
   exponential function. The period to sleep on the first backoff is the
   ``ms``; the ``multiplier`` is instead used to calculate the next
   backoff interval from the last. This is the default configuration.

-  ``{type = "None"}``: No sleep time between retries. This is the
   default value.

-  ``{type = "Fixed", ms = 1000 }``: A fixed amount of milliseconds to
   sleep between each retry attempt.

-  ``{type = "Variable", ms = [1000, 5000, 10000]}``: The amount of
   milliseconds between two consecutive retry attempts.

   The time to wait after 'i' retries is specified in the vector at
   position 'i'.

   If the number of retries is bigger than the vector length, then the
   last value in the vector is used. For example:

   ``ms = [111,222,333]`` -> It waits 111 ms after the first failure,
   222 ms after the second failure and then 333 ms for all following
   failures.

.. rubric:: Example of a complete Retry Strategy configuration

.. code:: toml

   [tornado.daemon]
   retry_strategy.retry_policy = {type = "Infinite"}
   retry_strategy.backoff_policy = {type = "Variable", ms = [1000, 5000, 10000]}

When not provided explicitly, the following default Retry Strategy is
used:

.. code:: toml

   [tornado.daemon]
   retry_strategy.retry_policy = {type = "MaxRetries", retries = 20}
   retry_strategy.backoff_policy = {type = "Exponential", ms = 1000, multiplier = 2 }
