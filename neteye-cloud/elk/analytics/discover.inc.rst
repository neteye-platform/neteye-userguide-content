
.. _elk-discover:

Discover
~~~~~~~

Discover lets you **search and explore indexed data** in detail. Use it to
look at raw documents, check what they contain and see how your data
changes over a selected time range. For a full description of every
feature, see the official Elastic documentation on
`Discover <https://www.elastic.co/docs/explore-analyze/discover>`_.

.. note::

   **Roles and permissions**

   * **VIEWER**: Read
   * **EDITOR**: Create, Read and Update
   * **ADMIN**: Create, Read and Update

With Discover you can:

* view the list of documents, the fields they contain and the details of
  each document, field by field
* see the trend over time in a histogram, together with the total number of
  matching documents
* limit the analysis to a specific period with the **time filter**
* **search** data with the search bar and narrow down the results with
  **filters**
* **sort** results on one or more fields and choose which columns to show
* choose the **data view** to work on
* inspect the request sent to Elasticsearch and the response it returns
* save searches to reuse later or to add to a dashboard

Data Views
``````````

A **data view** is a configuration that tells Kibana which Elasticsearch
data to use. It selects one or more indices or data streams and makes them
available for analysis in Discover, Dashboards and visualizations.

Every search in Discover runs on a single data view. To change it, open the
data view selector in the top-left corner of Discover and pick a data view
from the list. From the same menu you can also create a new data view by
giving it a name and an index pattern that matches the indices or data
streams you want to analyze.

Exploring your Data
```````````````````

**Time filter**
   Use the time picker at the top of the page to set the period to analyze,
   either as a relative range (for example, the last 15 minutes) or as
   absolute start and end dates. Use the refresh button to run the search
   again and show the latest results.

**Search bar and filters**
   Type a query in the search bar to find the documents you need. To narrow
   down the results, add filters next to the search bar, either on a
   specific field value or with a custom condition. Filters can be turned
   on and off, inverted or removed without changing the query.

**Histogram and document count**
   Above the results, a histogram shows how matching documents are spread
   over the selected time range, together with the total number of hits.
   Select a bar or drag across the chart to zoom into a shorter period.

**Document list**
   The main table lists the documents that match the current query, filters
   and time range. Expand a document to see all its fields and values.

**Field list**
   The sidebar on the left lists the fields available in the selected data
   view. From here you can:

   * add a field to the table as a column, or remove it
   * look at the most common values of a field
   * filter for or against a specific value

**Sorting and columns**
   Sort the results on one or more fields from the column headers, and
   rearrange or remove columns to show only the information you need.

Saving, Sharing and Inspecting Searches
```````````````````````````````````````

The toolbar at the top of Discover lets you:

* **Open** an existing saved search
* **Save** the current search, including the query, filters, selected
  columns and sorting, so you can reuse it later or add it to a dashboard
* **Share** the search with other users
* **Inspect** the search to see the request sent to Elasticsearch and the
  response it returned, which helps when troubleshooting unexpected results

.. note::

   Saving, updating and sharing searches requires the EDITOR or ADMIN role.
   VIEWER users can open and run existing saved searches.

Further reading:
`Discover in the Elastic documentation <https://www.elastic.co/docs/explore-analyze/discover>`_.
