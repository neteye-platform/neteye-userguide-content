How To Match on an Event With Dynamic OIDs
------------------------------------------

This How-To is intended to help you creating and configuring rules that
match Events, where part of a key is dynamic. In particular we're
looking at Snmptraps containing OIDs with an increasing counter as a
postfix.

This example shows a particular Snmptrapd Collector Event with dynamic
OIDs; However, it applies perfectly to any situation where it is
required to extract values from dynamically changing keys.

**Understanding the Use Case**

In some situations, Devices or Network Monitoring Systems emit SNMP
Traps, appending a progressive number to the OIDs to render them
uniquely identifiable. This leads to the generation of events with this
format:

.. code:: json

    {
       "type":"snmptrapd",
       "created_ms":"1553765890000",
       "payload":{
          "protocol":"UDP",
          "src_ip":"127.0.1.1",
          "src_port":"41543",
          "dest_ip":"127.0.2.2",
          "PDUInfo":{
             "version":"1",
             "notificationtype":"TRAP"
          },
          "oids":{
             "MWRM2-NMS-MIB::netmasterAlarmNeIpv4Address.20146578": {
                "content": "127.0.0.12"
             },
             "MWRM2-NMS-MIB::netmasterAlarmNeStatus.20146578": {
                "content": "Critical"
             }
          }
       }
    }

Here, the two entries in the ``oids`` section have a dynamic suffix
consisting of a number different for each event; in this specific event,
it is ``20146578``.

Due to the presence of the dynamic suffix, a simple path expression like
``${event.payload.oids."MWRM2-NMS-MIB::netmasterAlarmNeIpv4Address".content}``
would be ineffective. Consequently, we need a specific solution to
access the content of that changing key.

As we are going show, the solution consists of two steps: 1. Create a
Rule called ``my_extractor`` to extract the desired value from the
dynamic keys 2. Create a matching Rule that uses the extracted value

**Step #1: Creation of an extractor Rule**

To access the value of the
``MWRM2-NMS-MIB::netmasterAlarmNeIpv4Address.??????`` key, we will use
the ``single_key_match`` Regex extractor in the ``WITH`` clause.

The ``single_key_match`` extractor allows defining a regular expression
that is applied to the keys of a JSON object. If and only if there is
*exactly one* key matching it, the value associated with the matched key
is returned.

In our case the first rule is:

.. code:: json

    {
      "name": "my_extractor",
      "description": "",
      "continue": true,
      "active": true,
      "constraint": {
        "WHERE": null,
        "WITH": {
          "netmasterAlarmNeIpv4Address": {
            "from": "${event.payload.oids}",
            "regex": {
              "single_key_match": "MWRM2-NMS-MIB::netmasterAlarmNeIpv4Address.[0-9]+"
            }
          }
        }
      },
      "actions": []
    }

This rule: - has an empty ``WHERE``, so it matches every incoming event
- creates an extracted variable named ``netmasterAlarmNeIpv4Address``;
this variables contains the value of the OID whose key matches the
regular expression:
``MWRM2-NMS-MIB::netmasterAlarmNeIpv4Address.[0-9]+``

When the previously described event is received, the extracted variable
``netmasterAlarmNeIpv4Address`` will have the following value:

.. code:: json

    {
        "content": "127.0.0.12"
    }

From this point, all the rules in the same Ruleset that follows the
``my_extractor`` Rule can access the extracted value through the path
expression ``${_variables.my_extractor.netmasterAlarmNeIpv4Address}``.

**Step #2: Creation of the matching Rule**

We can now create a new rule that matches on the
``netmasterAlarmNeIpv4Address`` extracted value. As we are interested in
matching the IP, our rule definition is:

.. code:: json

    {
      "name": "match_on_ip4",
      "description": "This rule matches all events whose netmasterAlarmNeIpv4Address is 127.0.0.12",
      "continue": true,
      "active": true,
      "constraint": {
        "WHERE": {
          "type": "equals",
          "first": "${_variables.my_extractor.netmasterAlarmNeIpv4Address.content}",
          "second": "127.0.0.12"
        },
        "WITH": {}
      },
      "actions": []
    }

Now we have a rule that matches on the ``netmasterAlarmNeIpv4Address``
using a static path expression even if the source Event contained
dynamically changing OIDs.
