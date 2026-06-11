How To Use the Numerical Operators
----------------------------------

This How To is intended to help you configure, use and test rules
involving the numerical operators, allowing you to compare quantities in
a given Event either to each other or to a constant in the rule. One
important use case is in IoT, where you may be remotely measuring
temperatures, humidities, and other physically measurable quantities in
order to decide whether, for example, to gracefully shut down a server.

**Step #1: Simulating Rising Temperatures**

Because IoT hardware and its reporting software differ significantly
from one installation to another, we will simulate a rising series of
temperatures (Celsius) resulting in an action to shut down a server.

To do this, we will construct an Event that we can repeat, manually
changing the temperature each time:

.. code:: bash

   # curl -H "content-type: application/json" \
	  -X POST -vvv \
	  -d '{"event":{"type":"iot-temp", "created_ms":111, "payload": {"temperature":55, "ip":"198.51.100.11"}}, "process_type":"Full"}' \
	  http://localhost:4748/api/v1_beta/event/current/send | jq .

**Step #2: Configuring a Rule with Comparisons**

To start, let's create a rule that checks all incoming IoT temperature
events, extracts the temperature and source IP field, and if the
temperature is too high, uses the **Archive Executor** to write a
summary message of the event into a log file in a "Temperatures"
directory, then a subdirectory named for the source IP (this would allow
us to sort temperatures by their source and keep them in different log
directories). Given the "high temperature" specification, let's choose
the "greater than" operator:

+------------+----------------------------+
| Operator   | Description                |
+============+============================+
| gt         | Greater than               |
+------------+----------------------------+
| ge         | Greater than or equal to   |
+------------+----------------------------+
| lt         | Less than                  |
+------------+----------------------------+
| le         | Less than or equal to      |
+------------+----------------------------+

.. Eventually link to the Numerical Comparison documentation -->

All these operators can work with values of type Number, String, Bool,
null and Array, but we will just use Number for temperatures.

Now it's time to build our rule. The event needs to be both of type
**iot-temp** and to have its temperature measurement be greater than 57
(Celsius), which we will do by comparing the computed value of
**${event.payload.temperature}** to the number 57:

.. code:: json

    {
      "description": "This rule logs when a temperature is above a given value.",
      "continue": true,
      "active": true,
      "constraint": {
        "WHERE": {
          "type": "AND",
          "operators": [
            {
              "type": "equal",
              "first": "${event.type}",
              "second": "iot-temp"
            },
            {
              "type": "gt",
              "first":"${event.payload.temperature}",
              "second": 57
            }
          ]
        },
        "WITH": {}
      },
      "actions": [
        {
          "id": "archive",
          "payload": {
            "id": "archive",
            "payload": {
              "event": "At ${event.created_ms}, device ${event.payload.ip} exceeded the temperature limit at ${event.payload.temperature} degrees.",
              "archive_type": "iot_temp",
              "source": "${event.payload.ip}"
            }
          }
        }
      ]
    }

We'd like our rule to output a meaningful message to the archive log, for instance::

    At 17:43:22, device 198.51.100.11 exceeded the temperature limit at 59 degrees.


Our log message that implements :ref:`string interpolation
<tornado-string-interpolator>` should then have the following
template::

    ${event.created_ms}, device ${event.payload.ip} exceeded the temperature limit at ${event.payload.temperature} degrees.

So our rule needs to check incoming events of type *iot-temp*, and when
one matches, extract the relevant fields from the **payload** array.

..  Talk about specifics of how to change the rule to do what we want. -->

Remember to save our new rule where Tornado will look for active
rules, which in the default configuration is
*/neteye/shared/tornado/conf/rules.d/*. Let's give it a name like
*040_hot_temp_archive.json*.

Also remember that whenever you create a new rule and save the file in
that directory, you will need to restart the Tornado service. And it's
always helpful to run a check first to make sure there are no syntactic
errors in your new rule:

.. code:: bash

    # tornado --config-dir=/neteye/shared/tornado/conf check
    # systemctl restart tornado.service

**Step #3: Configure the Archive Executor**

.. We could use a link to the description of Archive Event. -->

If you look at the file
*/neteye/shared/tornado/conf/archive_executor.toml*, which is the
configuration file for the **Archive Executor**, you will see that the
default base archive path is set to
*/neteye/shared/tornado/data/archive/*. Let's keep the first part, but
under "[paths]" let's add a specific directory (relative to the base
directory given for "base_path"). This will use the keyword
"iot_temp", which matches the "archive_type" in the "action" part of
our rule from Step #2, and will include our "source" field, which
extracted the source IP from the original event's payload:::

    base_path =  "/neteye/shared/tornado/data/archive/"
    default_path = "/default/default.log"
    file_cache_size = 10
    file_cache_ttl_secs = 1

    [paths]
    "iot_temp" = "/temp/${source}/too_hot.log"

Combining the base and specific paths yields the full path where the log
file will be saved (automatically creating directories if necessary),
with our "source" variable instantiated. So if the source IP was
198.51.100.11, the log file's name will be::

    /neteye/shared/tornado/data/archive/temp/198.51.100.11/too_hot.log

Then whenever an IoT temperature event is received above the declared
temperature, our custom message with the values for time, IP and
temperature will be written out to the log file.

**Step #4: Watch Tornado "in Action"**

Let's observe how our newly configured temperature monitor works using a
bash shell. Open a shell and trigger the following events manually:

.. code:: bash

    # curl -H "content-type: application/json" \
           -X POST -vvv \
           -d '{"event":{"type":"iot-temp", "created_ms":111, "payload": {"temperature":55, "ip":"198.51.100.11"}}, "process_type":"Full"}' \
           http://localhost:4748/api/v1_beta/event/current/send | jq .
    # curl -H "content-type: application/json" \
           -X POST -vvv \
           -d '{"event":{"type":"iot-temp", "created_ms":111, "payload": {"temperature":57, "ip":"198.51.100.11"}}, "process_type":"Full"}' \
           http://localhost:4748/api/v1_beta/event/current/send | jq .

So far if you look at our new log file, you shouldn't see anything at
all. After all, the two temperature events so far haven't been greater
than 57 degrees, so they haven't matched our rule:

.. code:: bash

    # cat /neteye/shared/tornado/data/archive/temp/198.51.100.11/too_hot.log
    <empty>

And now our server has gotten hot. So let's simulate the next
temperature reading:

.. code:: bash

    # curl -H "content-type: application/json" \
           -X POST -vvv \
           -d '{"event":{"type":"iot-temp", "created_ms":111, "payload": {"temperature":59, "ip":"198.51.100.11"}}, "process_type":"Full"}' \
           http://localhost:4748/api/v1_beta/event/current/send | jq .

There you should see the full event written into the file we specified
during Step #2:

.. code:: bash

    # cat /neteye/shared/tornado/data/archive/temp/198.51.100.11/too_hot.log
    At 17:43:22, device 198.51.100.11 exceeded the temperature limit at 59 degrees.

 Wrapping Up

That's it! You've successfully configured Tornado to respond to high
temperature events by logging them in a directory specific to
temperature sensor readings for each individual network device.

You can also use different executors, such as the **Icinga 2 Executor**,
to send IoT events as monitoring events straight to Icinga 2 where you
can see the events in a NetEye dashboard. The `Icinga
documentation <https://icinga.com/docs/icinga2/latest/doc/12-icinga2-api/#actions>`__
shows you which commands the executor must implement to achieve this.
