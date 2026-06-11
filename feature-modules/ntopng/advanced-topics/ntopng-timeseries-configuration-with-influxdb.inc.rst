
Using ntopng with InfluxDB
~~~~~~~~~~~~~~~~~~~~~~~~~~

ntopng can be configured to work with InfluxDB to write and read
timeseries data. To configure it, follow the following steps.

1. Open ntopng from NetEye (``Sidebar menu >> ntopng``) and then, in
   ntopng click on the ``Settings >> Preferences`` option in the left
   menu bar.
2. Now, go to **timeseries** preference option and configure these
   settings:

   -  Timeseries Driver: *InfluxDB 1.x*
   -  InfluxDB Url: *https://<influxdb-domain>:8086* (use HTTPS)
   -  InfluxDB Database: *<database-name>* i.e. ntopng

   In NetEye, InfluxDB is exposed via HTTPS and access requires username/password authentication.
   When configuring `ntopng`, always use an `https://` URL and enable InfluxDB Authentication
   in order to enter valid credentials.

3. If you are using authentication credentials to secure InfluxDB,
   *enable* the ``InfluxDB Authentication`` option and add the
   credentials. Default value is *disable*.

4. Configure the timeseries options (i.e., Interface Timeseries, Local
   Hosts Timeseries, Devices Timeseries and Other Timeseries) according
   to your preferences

5. Click on ``Save`` button to save the preference configuration.

Once done, you will see the folder created under the
``/neteye/shared/influxdb/data/data/`` location. Moreover, logs
written successfully will be available when you run ``journalctl -u
influxdb -f``, like in this example::

    Aug 31 11:02:31 lenovo31 influxd[476]: [httpd] ::1 - - [31/Aug/2020:11:02:31 +0200] "POST /write?db=ntopng  HTTP/1.1" 204 0 "-" "-" 2bd62e84-76f0-11e9-801c-f0761cfbf2d8 8555

The official documentation of ntopng timeseries with
influxdb <https://www.ntop.org/guides/ntopng/basic_concepts/timeseries.html#influxdb-driver>`__
contains more information about the preference's configuration.

Configuring retention for ntopng time series in InfluxDB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The retention of ntopng time series written to InfluxDB can be configured directly
from the ntopng UI.

Go to `Settings` → `Preferences` → `Timeseries`, locate the retention / data retention
settings for timeseries, set the desired retention, and select `Save`.

ntopng will apply the retention settings to the timeseries data stored in InfluxDB.

.. note:: If ntopng cannot apply retention changes due to insufficient InfluxDB permissions,
   adjust the InfluxDB user privileges accordingly.


Create a Grafana data source to access time series data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ntopng time-series historical data stored in the *InfluxDB* can also
be used by *ITOA* module to display the real-time network traffic and
flow collection performance metrics.

Before that, you need to configure a new data source in *Grafana* to
access that historical data. To configure it, follow the following
steps.

1. Open **ITOA Dashboard** in *NetEye* and then select **Data Sources**
   option from the **configuration** menu.

2. Click on the *Add data source* button and then select **InfluxDB**
   from the **Time series database** list.

3. Now, configure these settings:

   * Name: *<datasource-name>* i.e ntop-influx
   * URL: *https://<influxdb-domain>:8086*
   * Database: *<database-name>* i.e. ntopng
   * User: *admin*

.. rubric:: Authentication

Since InfluxDB is configured with authentication, enable **Basic auth** in
the Grafana datasource configuration and set:

* Basic Auth User: `<influxdb-username>`
* Basic Auth Password: `<influxdb-password>`

.. note:: The *InfluxDB URL* and *Database* name should be exactly the
   same as you used in ntopng (``ntopng >> Settings >> Preferences >>
   Timeseries``)

Troubleshooting
~~~~~~~~~~~~~~~

If the ``<database-name>`` folder does not exists in the specified
location ``/neteye/shared/influxdb/data/data/`` or if *POST /write*
messages are not available, then from the *ntopng timeseries
preferences*, switch the *timeseries driver* to **RRD**, then back to
**InfluxDB** and restart the *ntopng* service.
