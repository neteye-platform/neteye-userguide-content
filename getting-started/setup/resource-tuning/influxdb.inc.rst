InfluxDB
~~~~~~~~

InfluxDB is a time series database designed to handle high volumes of
write and query loads in NetEye. If you want to learn more about
InfluxDB you can refer to the official `InfluxDB documentation
<https://docs.influxdata.com/influxdb/>`__

.. rubric:: Migration of inmem (in-memory) indices to TSI (time-series)

From NetEye 4.14, InfluxDB will use the Time Series Index (TSI).

However, the existing setup will still use the TSM index for writing and
fetching data until you perform the migration procedure, which consists
of the following steps.

1. Build TSI by running the ``influx_inspect buildtsi`` command:

   In a cluster environment, the below command must be executed on the
   node on which the *InfluxDB* resource is running::

      sudo -u influxdb influx_inspect buildtsi -datadir /neteye/shared/influxdb/data/data -waldir /neteye/shared/influxdb/data/wal -v

   Upon execution, the above command will build TSI for **all** the
   databases that exist in *

   .. note:: If you want to build TSI only for a specific database
      then add the ``-database <database_name>`` parameter to the above
      command.

2. Restart the ``influxdb`` service:

-  Single node::

      systemctl restart influxdb

-  Cluster environment::

      pcs resource restart influxdb

The official documentation of `InfluxDB Upgrade
<https://docs.influxdata.com/influxdb/v1.8/administration/upgrading/>`__
contains more information about the inmem (in-memory) to TSI
(time-series) migration process.
