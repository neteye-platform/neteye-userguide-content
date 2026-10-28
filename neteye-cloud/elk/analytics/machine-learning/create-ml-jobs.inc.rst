.. _elk-ml-create-jobs:

Creating Machine Learning Jobs
------------------------------

Machine learning analyses in Kibana run as **jobs**. This chapter explains
how to create a generic machine learning job from the Machine Learning section,
and how to create an anomaly detection job for APM services from
Observability.

.. _elk-ml-create-job:

Create a Machine Learning Job
`````````````````````````````

You can create a job for anomaly detection or for data frame analytics. The
exact steps depend on the type of analysis, but the overall workflow is the
same. The procedure below uses anomaly detection as an example.

#. In Kibana, go to *Analytics > Machine Learning*.
#. Choose the type of analysis, **Anomaly detection** or
   **Data frame analytics**, and open its job management page. This page
   lists the existing jobs and their details.

   .. figure:: /neteye-cloud/elk/analytics/img/anomaly-detection-jobs.png
      :alt: Anomaly Detection Jobs page showing the job list and Create job button
      :align: center

#. Click **Create job**.
#. Select the data source you want to analyze.
#. Choose how to configure the job:

   * **Use a preconfigured job**: if the selected data matches a known data
     pattern or integration, Kibana suggests ready-to-use job configurations
     for common use cases. This is the quickest option.
   * **Use a wizard**: choose the wizard that matches your use case (see
     :ref:`elk-ml-job-wizards`).
   * **Explore your data first**: if you are not sure which type of job to
     create, open the Data Visualizer to check which fields are suitable
     for machine learning analysis.

   .. figure:: /neteye-cloud/elk/analytics/img/create-job-from-data-view.png
      :alt: Job creation page showing preconfigured jobs, job wizards, and the Data Visualizer
      :align: center

#. Configure the analysis settings for the selected job type.
#. Define the job details.
#. Review the configuration.
#. Create and start the job.

For a complete tutorial, see the official Elastic guide
`Getting started with anomaly detection
<https://www.elastic.co/docs/explore-analyze/machine-learning/anomaly-detection/ml-getting-started>`_.

.. _elk-ml-job-wizards:

Anomaly Detection Wizards
^^^^^^^^^^^^^^^^^^^^^^^^^

Each wizard is designed for a different anomaly detection scenario:

Single metric
   Detects anomalies in a single time series.

Multi-metric
   Detects anomalies in one or more metrics, with the option to split the
   analysis.

Population
   Detects unusual behavior across a population, especially for
   high-cardinality data.

Advanced
   Gives access to the full set of configuration options for more complex
   use cases.

Categorization
   Groups log messages into categories and detects anomalies within them.

Rare
   Detects rare values or infrequent events in time series data.

Geo
   Detects anomalies in geographic location data.

.. _elk-ml-create-apm-job:

Create an Observability Machine Learning Job
````````````````````````````````````````````

An APM anomaly detection job learns the normal behavior of your application
services over time. It then highlights significant deviations that may
point to performance issues, traffic anomalies or unusual failure patterns.

Compared with manual monitoring, static thresholds or generic ML jobs, an
APM job:

* automatically detects abnormal service behavior
* makes potential issues visible earlier
* helps identify unusual changes in latency, throughput or failed
  transaction rate
* needs less effort to configure

To create an APM anomaly detection job:

#. In Kibana, go to *Observability > Service inventory > Settings*.

   .. figure:: /neteye-cloud/elk/analytics/img/observability-service-inventory.png
      :alt: Observability Service inventory page with the Settings link highlighted
      :align: center

#. Select **Anomaly detection** and click **Create job**.
#. Choose the environments to create the job for.

The job is then created and appears in the job list. From the job list you
can open the job settings and view the job results.

To understand the results, see :ref:`elk-ml-job-results`.
