.. _tornado-snmptrap-collector:

SNMP Trap Event
~~~~~~~~~~~~~~~

The SNMP Trap Collector receives and parses messages coming from snmptrapd.
It will then pass those messages as Events through a specific communication channel to Tornado.

The snmptrap is configured out of the box on the |ne| Master and, if present, on Satellites.
Thus, in case a trap message received by a tenant will be automatically sent to Tornado via through
its dedicated Satellite via NATS Communication channel.

The received messages are kept in an in-memory non-persistent buffer
that makes the application resilient to crashes or temporary
unavailability of the communication channel. When the connection to the
channel is restored, all messages in the buffer will be sent. When the
buffer is full, the Collectors will start discarding old messages. The
buffer max size is set to ``10000`` messages.

Consider a snmptrapd message that contains the following information::

   PDU INFO:
     version                        1
     errorstatus                    0
     community                      public
     receivedfrom                   UDP: [127.0.1.1]:41543->[127.0.2.2]:162
     transactionid                  1
     errorindex                     0
     messageid                      0
     requestid                      414568963
     notificationtype               TRAP
   VARBINDS:
     iso.3.6.1.2.1.1.3.0            type=67 value=Timeticks: (1166403) 3:14:24.03
     iso.3.6.1.6.3.1.1.4.1.0        type=6  value=OID: iso.3.6.1.4.1.8072.2.3.0.1
     iso.3.6.1.4.1.8072.2.3.2.1     type=2  value=INTEGER: 123456

The Collector will produce this Tornado Event:

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
            "errorstatus":"0",
            "community":"public",
            "receivedfrom":"UDP: [127.0.1.1]:41543->[127.0.2.2]:162",
            "transactionid":"1",
            "errorindex":"0",
            "messageid":"0",
            "requestid":"414568963",
            "notificationtype":"TRAP"
         },
         "oids":{
            "iso.3.6.1.2.1.1.3.0":"67",
            "iso.3.6.1.6.3.1.1.4.1.0":"6",
            "iso.3.6.1.4.1.8072.2.3.2.1":"2"
         }
      }
   }

The structure of the generated Event is not configurable.
