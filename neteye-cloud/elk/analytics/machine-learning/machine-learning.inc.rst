Machine Learning Overview
-------------------------

Machine Learning in Kibana automatically detects anomalies, unusual patterns
and significant changes in behavior in your data. It analyzes how data evolves
over time, models what normal behavior looks like and highlights values,
events or trends that differ from the expected pattern.

With manual analysis, you have to inspect charts, logs or metrics yourself.
Machine Learning finds hidden or unexpected behavior automatically, even in
large and complex datasets. This is especially useful when there is too much
data to inspect visually, or when static thresholds alone can't reliably
identify abnormal behavior.

With Machine Learning, you can:

* **Detect anomalies** in indexed data
* **Identify unusual changes** in trends, counts or metric values
* **Analyze patterns** that are difficult to detect manually
* **Support monitoring and investigation activities** with automated insights

Machine Learning takes you beyond simply observing data to a more proactive
form of analysis, where you can detect relevant deviations faster and
investigate them more efficiently.

.. rubric:: Roles and Permissions

Depending on the role defined by the contract with your organization, you might have different permissions:

* **VIEWER**: Read
* **EDITOR**: Read
* **ADMIN**: Read

Kibana supports two types of machine learning: unsupervised and supervised.

Unsupervised Machine Learning
`````````````````````````````

Unsupervised learning doesn't require a training phase. The system detects
patterns or unusual behavior directly from the data. The main approaches are
**anomaly detection** and **outlier detection**.

* `Anomaly detection <https://www.elastic.co/docs/explore-analyze/machine-learning/anomaly-detection>`_
  works with **time series data**. It builds a probabilistic model of normal
  behavior and can run continuously to detect unusual events as they occur.
  Because the model evolves over time, it can also help you forecast future
  behavior.

* `Outlier detection <https://www.elastic.co/docs/explore-analyze/machine-learning/data-frame-analytics/ml-dfa-finding-outliers>`_
  doesn't require time series data. It is a **data frame analytics**
  technique that identifies unusual records by comparing each data point with
  the others and evaluating how isolated it is within the dataset. The result
  is an enriched copy of the dataset in which each record has an
  **outlier score** that shows how abnormal it is compared to the rest of the
  data.

Supervised Machine Learning
```````````````````````````

Supervised learning requires a **training dataset**. In Kibana, the main
supervised data frame analytics types are **classification** and
**regression**.

Both types of analysis produce:

* A trained model
* An enriched copy of the dataset, with predictions added to each record

You can then deploy the trained model to generate predictions on new data.

* `Classification <https://www.elastic.co/docs/explore-analyze/machine-learning/data-frame-analytics/ml-dfa-classification>`_
  predicts a **category or class**. The model learns from historical data in
  which the expected outcome is already known. It then uses what it learned
  to assign new records to one of the possible classes. Use classification
  when the result is a label rather than a number, for example to decide
  whether:

  * An event is normal or suspicious
  * A transaction should be approved or rejected
  * A DNS request comes from a malicious or a benign domain

* `Regression <https://www.elastic.co/docs/explore-analyze/machine-learning/data-frame-analytics/ml-dfa-regression>`_
  predicts a **numerical value**. The model learns the relationship between
  the available fields and a continuous target value, then estimates that
  value for new records. Use regression when the expected result is a
  measurable quantity, for example to estimate:

  * The response time of a web request
  * A cost
  * A duration or usage level

For more information, see the official
`Elastic Machine Learning documentation <https://www.elastic.co/docs/explore-analyze/machine-learning>`_.
