NetEye can be set up to send SMS notification to allow DevOps and
Network Managers to be informed immediately about problems in the
monitored infrastructure and promptly take adequate actions.

It is not always possible to connect an SMS Gateway between NetEye and a
physical NetEye appliance like an SMS modem. For instance, you may be in
a situation where:

* NetEye is operated on virtual infrastructure (e.g., the Cloud or
  with VMware)

* The mobile network signal is weak, forcing the SMS Gateway to be
  located at a distance that exceeds the maximum length for a serial
  cable

A dedicated `SMS LAN Gateway` (i.e., a serial-to-ethernet adaptor) that can
solve these problems is available for use with NetEye. These devices
are sourced from `Moxa
<https://www.moxa.com/en/products/industrial-edge-connectivity/serial-device-servers/terminal-servers/nport-6100-6200-series>`__
and are tested for compatibility with the notification strategy of
NetEye.

If you are interested in the SMS LAN Gateway, please contact `the
NetEye support <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portals>`__.

How It Works
~~~~~~~~~~~~

With the Moxa device in *TCP Server Mode*, the host computer (NetEye)
initiates contact with the NPort 6150, establishes the connection, and
receives data from the serial device. This operational mode also
supports up to 8 simultaneous bidirectional TCP/IP connections, enabling
multiple hosts to collect data from the same serial device at the same
time.

The Moxa NPort 6150 supports SSL and SSH, encrypting data before sending
it over the network. It has port buffers for storing serial data when
the Ethernet connection is down, and the serial connection supports
RS-232, RS-422 and RS-485.

NPort 6150 Hardware Setup
~~~~~~~~~~~~~~~~~~~~~~~~~

We recommend that you assign a static IP for the Moxa NPort device
before connecting it.

You should first set up both hardware devices before proceeding to
configure the software. The basic steps are:

#. Set up the Moxa NPort device and power it on
#. Check that the Ready and Link LEDs are green
#. Connect the modem to the Moxa NPort device with an RS-232 serial
   cable, attach an antenna, insert the SIM, and power it on
#. Check that the modem's LED changes from red to green
#. Connect the Moxa NPort device to the network with an Ethernet cable

Moxa Device Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~

The Moxa NPort series has both a built-in telnet server and a built-in
web server for configuration. You can use either method as they have the
same functionality. You should change the IP address, netmask and
gateway according to your needs. Other changes are not necessary as we
connect our Wavecom GSM Gateway to the Serial interface on the adaptor
and the default settings work without problems.

The device is configured at the factory with the following default
profile:

    IP address:  192.168.127.254
    Username:    admin
    Password:    <blank or "moxa">

Once connected, you will be asked to change your password (press ESC to
skip this). To change the default IP address, go to the Network tab and
then the Basic tab. Change the field for the second line marked "IPv4
address", and change the netmask and gateway if necessary. To change the
serial interface type from the default RS-232, go to **Port > Line >
Interface**.

Configuration for NetEye
~~~~~~~~~~~~~~~~~~~~~~~~

You will need to install the modules without updating the kernel
itself.

.. code:: bash

   # yum install moxa-npreal2 kmod-npreal2

If you connect the Moxa NPort device to a remote switch on the network,
you should skip this next step. However, if you connect your Moxa device
directly to a physical NetEye server on a dedicated ethernet port, you
must configure the dedicated network interface on NetEye with the
desired network configuration.

Next, configure the IP address of the NPort 6150 for the Linux driver
(this step is necessary since the Moxa NPort device is connected to
NetEye via Ethernet). After configuring *npreals*, you will need to
return here to insert the number appended to the end of the IP address
(see below for which value to use):

.. code:: bash

   # cat /etc/sysconfig/npreals

.. container:: codeblock

   .. code::

      # Configure devices in this mode:
      # DEVICES="IP:PORT(s) {IP:PORT(s)}"
      #
      DEVICES="192.168.127.254:1"

Start and Test the Driver Daemon
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set up the *npreals* system service.

.. code:: bash

   # systemctl enable npreals.service
   # systemctl start npreals

Next, you will need to manually install the UUCP package, which is used
to communicate with the modem.

.. code:: bash

   # yum install http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/u/uucp-1.07-41.el7.x86_64.rpm

Now get the device's *tty* name from the *npreals* configuration, which in
this example, it is *ttyr00*,

.. code:: bash

   # cat /usr/lib/npreal2/driver/npreal2d.cf

.. container:: codeblock

   .. code::

    #=========================================================#
    #   This configuration file is created by Moxa NPort      #
    #   Administrator Program automatically, please do not    #
    #   modify this file by yourself.                         #
    #=========================================================#
    ttymajor=3
    3htmclalloutmajor=38
    #[Minor] [ServerIP] [data]  [cmd]   [FIFO]  [SSL]   [ttyName] [coutName] [interface][mode][BackIP]
    0   192.168.127.254 950 966 1   0   ttyr00  cur00   0   0   (null)

You MUST use this *tty* name in the modem's *smsd.conf* file under the
section **[GSM1] > device** (here *ttyr00* instead of *ttyS0* as
:ref:`described in the Modem Setup section <sms-modem-setup>`). In
addition, check that the number in the [FIFO] field is the number
after the colon in the file */etc/sysconfig/npreals* as shown further
above.

Next, test your access to the Moxa NPort device with the device's *tty*
name from the prior step using the **cu** terminal program.

.. code:: bash

   # ping 192.168.127.254

.. container:: codeblock

   .. code::

    ...

.. code:: bash

   # cu -s 115200 -l /dev/ttyr00

.. container:: codeblock

   .. code::

    Connected.
    ~.
    Disconnected.

If the connection was successful (i.e., you see the "Connected."
response), you can now proceed to configure the GSM modem interface
and test sending SMS messages :ref:`as described in the setup
<sms-modem-setup>`. Note that to end a **cu** session you will need to
enter the command **~.** since the terminal program will block all
control characters that typically end a session.

If instead you see a message such as:

.. code::

    cu: /dev/ttyr00: Line in use

then double check your configuration in *smsd.conf* (especially the
*tty* device) and the permissions as described below.

Final Notes
~~~~~~~~~~~

If you want to use *smssend*, remember that that script uses a different
embedded default path which you will need to change:

.. code::

    # /usr/bin/smssend
