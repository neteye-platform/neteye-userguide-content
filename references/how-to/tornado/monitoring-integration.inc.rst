.. _tornado-howto-monitoring-executor:

How To Use the Monitoring Executor for setting statuses on any object
---------------------------------------------------------------------

This How To is intended to help you configure, use, and test the
**Monitoring Executor**, a new Tornado feature that allows to
automatically create new hosts, services, or both in case an event
received by Tornado refers to objects not yet known to Icinga and the
Director. The *Monitoring Executor* performs a **process-check-results**
on Icinga Objects, creating hosts and services in both Icinga and the
Director if they do not exist already.

**Requirements for the Monitoring Executor**

A setup is needed on both Tornado and Icinga Director / Icinga 2:

*Tornado*

In a production environment, Tornado can accept events from any
collector and even via an HTTP POST request with a JSON payload; we will
however show how to manually send an event to Tornado from the GUI to
let a suitable rule to fire and create the objects.

To allow for a correct interaction between Tornado and Icinga 2 / Icinga
Director, you need to make sure that the corresponding username and
password are properly set to your dedicated Tornado user in the files::

    /neteye/shared/tornado/conf/icinga2_client_executor.toml
    /neteye/shared/tornado/conf/director_client_executor.toml

.. note:: If one or both passwords are empty, probably you need to
   execute the :command:`neteye install` script, that will take care
   of setting the passwords.

*Icinga Director / Icinga 2*

In Icinga, create a host template and a service template that we will
then use to create the hosts, so make sure to write down the name given
to the two templates.

-  Create a **host template** called *host_template_example* with the
   following properties:

   -  Check command: *dummy*
   -  Execute active checks: *No*
   -  Accept passive checks: *Yes*

-  Create a **service template** called *service_template_example*
   with the following properties:

   -  Check command: *dummy*
   -  Execute active checks: *No*
   -  Accept passive checks: *Yes*

-  Deploy this configuration to Icinga 2

**Scenario**

We will set up a rule that intercepts an event sent by the SNMPtrapd
Collector, so let's suppose that Tornado receives the following event.

.. code:: json

    {
      "created_ms": 1000,
      "type": "snmptrapd",
      "payload": {
        "src_port": "9999",
        "dest_ip": "192.168.0.1",
        "oids": {
          "SNMPv2-MIB::snmpTrapOID.0": {
            "datatype": "OID",
            "content": "ECI-ALARM-MIB::eciMajorAlarm"
          },
          "ECI-ALARM-MIB::eciLSNExt1": {
            "content": "ALM_UNACKNOWLEDGED",
            "datatype": "STRING"
          },
          "ECI-ALARM-MIB::eciObjectName": {
            "content": "service.example.com",
            "datatype": "STRING"
          }
        },
        "src_ip": "172.16.0.1",
        "protocol": "UDP",
        "PDUInfo": {
          "messageid": 0,
          "notificationtype": "TRAP",
          "version": 1,
          "errorindex": 0,
          "transactionid": 1,
          "errorstatus": 0,
          "receivedfrom": "UDP: [172.16.0.1]:9999->[192.168.0.1]:4444",
          "community": "public",
          "requestid": 1
        }
      }
    }

We want to use the payload of this event to carry out two actions:

1. Set the status of a service
2. create the host and service if the do not exist already.

To do so, we will build a suitable rule in Tornado, that defines the
conditions for triggering the actions and exploits the *Monitoring
Executor*'s ability to interact with Icinga to actually carry them out.

.. warning:: When you create your own rules, please pay attention to
   the correct escaping of the SNMP strings, or the rule might not
   fire correctly!

**Step #1: Define the Rule's Conditions**

This first task is fairly simple: Given the event shown in the Scenario
above, we want to set a **critical** status on the service specified in
``payload -> oids -> ECI-ALARM-MIB::eciObjectName -> content``, when
**both** these conditions are met:

#. ``payload -> oids -> SNMPv2-MIB::snmpTrapOID.0 -> content`` is
   equal to ``ECI-ALARM-MIB::eciMajorAlarm``
#. ``payload -> oids -> ECI-ALARM-MIB::eciLSNExt1 -> content`` is
   equal to ``ALM_UNACKNOWLEDGED``

We capture all these conditions by creating the following rule in the
Tornado GUI (you can set the not mentioned options at your preference):

-  *Name*: choose the name you prefer
-  *Description*: a string like *Set the critical status for snmptrap*
-  *Constraint*: you can copy and paste it for simplicity:

.. code::

    "WHERE": {
        "type": "AND",
        "operators": [
            {
              "type": "equals",
              "first": "${event.type}",
              "second": "snmptrapd"
            },
            {
              "type": "equals",
              "first": "${event.payload.oids.\"SNMPv2-MIB::snmpTrapOID.0\".content}",
              "second": "ECI-ALARM-MIB::eciMajorAlarm"
            },
            {
              "type": "equals",
              "first": "${event.payload.oids.\"ECI-ALARM-MIB::eciLSNExt1\".content}",
              "second": "ALM_UNACKNOWLEDGED"
            }
        ]
    },

**Step #2: Define the Actions**

Now that we have defined the rule's conditions, we want the rule to
trigger the two actions

1. a Monitoring action that will set the **critical** status for the
   service specified in
   ``payload -> oids -> ECI-ALARM-MIB::eciObjectName -> content``
2. if necessary, create the underlying host and service in Icinga2 and
   in the Director

To do so we need to configure the payload of the Monitoring action as
follows (check the snippet below).

-  **action_name** must be set to
   ``create_and_or_process_service_passive_check_result``

-  **process_check_result_payload** must contain:

   -  the **service** on which to perform the process-check-result,
      using the ``<host_name>!<service_name>`` notation
   -  **exit_status** for a *critical* event, which is equal to **2**
   -  a human readable **plugin_output**

   The fields contained in this payload correspond to those that are
   used by the Icinga2 APIs, therefore you can also provide more data in
   the payload to create more precise rules.

-  **host_creation_payload** takes care of creating the underlying
   host with the following properties:

   -  a (hard-coded) host name (although it can be configured from the
      payload of the event)
   -  the host template we created in the **Requirements** section
      (i.e., *host_template_example*)

-  **service_creation_payload** takes care of creating the underlying
   service with the following properties:

   -  the name of the service as provided in
      ``payload -> oids -> ECI-ALARM-MIB::eciObjectName   -> content``
   -  the service template we created in the **Requirements** section
      (i.e., *service_template_example*)
   -  the host as the one specified in ``host_creation_payload``

.. warning:: In *process_check_result_payload* it is mandatory to
   specify the object on which to perform the process-check-result
   with the field **"service"** (or "host", in case of check result on
   a host). This means that for example specifying the object with the
   field **"filter"** is **not** valid

The above actions can be written as the following JSON code, that you
can copy and paste within the **Actions** textfield of the rule. Make
sure to maintain the existing square brackets in the textfield!

 .. code:: json

    {
      "id": "monitoring",
      "payload": {
        "action_name": "create_and_or_process_service_passive_check_result",
        "process_check_result_payload": {
          "exit_status": "2",
          "plugin_output": "CRITICAL - Found alarm ${event.payload.oids.\"SNMPv2-MIB::snmpTrapOID.0\".content}",
          "service": "acme-host!${event.payload.oids.\"ECI-ALARM-MIB::eciObjectName\".content}",
          "type": "Service"
        },
        "host_creation_payload": {
          "object_type": "Object",
          "object_name": "acme-host",
          "imports": "host_template_example"
        },
        "service_creation_payload": {
          "object_type": "Object",
          "host": "acme-host",
          "object_name": "${event.payload.oids.\"ECI-ALARM-MIB::eciObjectName\".content}",
          "imports": "service_template_example"
        }
      }
    }

Now we can save the rule and then deploy the Rule.

**Step #3: Send the Event**

With the rule deployed, we can now use the Tornado GUI's *test window*
to send a payload and test the rule.

In the test window, add *snmptrapd* as **Event Type** and paste the
following code as **Payload**. Now, with the **Enable execution of
actions** *disabled*, click on the **Run test** button. If everything is
correct, you will see a **MATCHED** string appear on the left-hand side
of the rule's name. Now, enable the execution of actions and click again
on the **Run test** button.

Now, on the NetEye GUI you should see that in both Icinga 2 **and** in
the Director:

-  a new host **unknown-host** was created--in Icinga 2 it will have
   state **pending** since we did no checks on it

-  a new service **service.example.com** was created--in Icinga 2 with
   state **critical** (may be SOFT, depending on your configuration)

**Conclusions**

In this how-to we created a specific rule for setting the **critical**
status of a non-existing service. Of course with more information on the
incoming events we may want to add different rules so that Tornado can
set the status of the service to **ok** for example, when another event
with different content arrives.
