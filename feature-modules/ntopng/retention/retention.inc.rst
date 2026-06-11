

Data Retention for ntopng
~~~~~~~~~~~~~~~~~~~~~~~~~

ntopng by default keeps the flows and alerts data for `7 days`.
However, you can configure it from ntopng UI in the following preferences section:
:menuselection:`Settings / Preferences / Expert View / Data Retention`.

ClickHouse Log Retention
~~~~~~~~~~~~~~~~~~~~~~~~

ClickHouse stores logs in `System` tables. Due to the amount of information stored,
they can take up a large amount of disk space.
For this reason the default retention set on all ClickHouse `System` tables is `7 days`.
You can change the retention value by following these steps:

* create a custom sysconfig environment file at:
  :file:`/neteye/local/clickhouse-server/conf/sysconfig/clickhouse-server-user-customization`
* specify the new retention value for each table the user wants to customize in the format

   .. code:: bash

     QUERY_LOG_RETENTION="event_date + INTERVAL 5 DAY"

  The available variables are

  * `QUERY_LOG_RETENTION`: to set the retention of the `query_log` table, which contains information about the executed queries
  * `TRACE_LOG_RETENTION`: retention of the table containing stack traces of the query profiler
  * `QUERY_THREAD_LOG_RETENTION`: retention of the table containing information about the threads executing the single queries
  * `QUERY_VIEWS_LOG_RETENTION`: retention of the table containing information about the views executed when running a certain query
  * `PART_LOG_RETENTION`: retention of the table containing information about table parts events, namely events connected with tables using the `MergeTree` engine
  * `METRIC_LOG_RETENTION`: retention of the table containing the history of metric values from other system tables
  * `ASYNC_METRIC_LOG_RETENTION`: retention of the table containing the historical values of some asynchronous metrics calculated in background

* restart the ClickHouse service: :command:`systemctl restart clickhouse-server.service`

.. seealso:: `ClickHouse system tables <https://clickhouse.com/docs/en/operations/system-tables/>`__ for more information.
