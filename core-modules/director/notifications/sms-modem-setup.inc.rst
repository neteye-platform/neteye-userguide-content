.. _sms-modem-setup:

SMS Modem Setup
~~~~~~~~~~~~~~~

NetEye uses SMS Server Tools 3 (smstools) for handling the GSM modem
interface. More detailed configuration documentation can be found in
the `smstools official documentation
<http://smstools3.kekekasvi.com/index.php?p=configure>`_).

The configuration file is located at
*/neteye/local/smsd/conf/smsd.conf*.

Here is a sample smsd.conf file::

    # Example smsd.conf.

    devices = GSM1
    #logfile = 1
    logfile = /neteye/local/smsd/log/smstools.log
    # loglevel [1-7]:
    # A higher number indicates increased verbosity.
    loglevel = 7

    # PLEASE DO NOT EDIT THESE PATHS
    failed = /neteye/local/smsd/data/spool/failed
    incoming = /neteye/local/smsd/data/spool/incoming
    checked = /neteye/local/smsd/data/spool/checked
    outgoing = /neteye/local/smsd/data/spool/outgoing
    executable_check = no


    [GSM1]
    # Use the modem's tty, or the tty of a bridging device if you are using one
    device = /dev/ttyS0
    # For older WaveCom modem devices, use this baudrate:
    #baudrate = 9600
    # For newer WaveCom and Sierra devices, use this instead:
    baudrate = 115200
    # For the new Sierra FX30(S) modem, uncomment this line:
    #rtscts = no
    mode = new
    incoming = yes
    cs_convert = yes
    # Uncomment this line if your SIM has a pin (we recommend leaving the SIM PIN blank):
    #pin = 1111
    eventhandler = /usr/lib64/tornado/bin/tornado_sms_collector -c /neteye/shared/tornado_sms_collector/conf/


.. note:: For `Sierra FX30
   <https://source.sierrawireless.com/devices/fx-series/fx30/>`_ and
   FX30S models, remember to uncomment the parameter **rtscts = no**
   above. Also, if you are using the Moxa NPort 6150 to :ref:`extend your
   modem's range <sms-gateway-moxa>`, be sure to insert the Moxa *tty*
   device in place of *ttyS0*.

After changing the configuration, you will need to restart the SMS
daemon as follows::

    [root@neteye ~]# systemctl restart smsd

Testing SMS Notifications
`````````````````````````

The phone number should include the country code and contain only
numbers. So for instance a phone number in Italy might be
**00391234567890**.

There are two methods for testing that SMS messages are correctly sent:

#. Send an SMS message directly from the command line with the *smssend*
   script: ``# /usr/bin/smssend 00391234567890 "TEST FROM NETEYE"``
#. Interact directly with the *smsd* daemon. To do this, create a file
   with content like the following in
   /neteye/local/smsd/data/spool/outgoing/ (the actual name of the file
   doesn't matter). The *smsd* daemon will send the SMS without further
   intervention::

      To: 00391234567890

      TEST FROM NETEYE

To check the status, you can look directly in the directories under
*/neteye/local/smsd/data/spool/* or check the log file, for instance
with::

    # tail -f /neteye/local/smsd/log/smstools.log
