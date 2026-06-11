.. _tornado-sms-collector:

SMS
~~~

Before Tornado can correctly catch SMS, you should configure your
SMS modem to send events to Tornado. You can follow the configuration
procedure at :ref:`sms-modem-setup`.

Tornado SMS Collector will then receive then valid SMS like:

.. code::

   From: 39123456789
   From_TOA: 91 international, ISDN/telephone
   From_SMSC: 39123456789
   Sent: 23-10-09 16:06:52
   Received: 23-10-09 16:06:58
   Subject: GSM1
   Modem: GSM1
   IMSI: 222018005102877
   Report: no
   Alphabet: ISO
   Length: 20

   Example text message

and based on that will create the following Tornado Event:

.. code:: json

   {
     "type": "sms",
     "created_ms": 155412314854,
     "payload": {
        "modem":"GSM1",
        "sender": "+39123456789",
        "text": "Example text message"
     }
   }

Within the Tornado Event, the **modem**, **sender** and **text** properties
are the values extracted from the incoming SMS.
