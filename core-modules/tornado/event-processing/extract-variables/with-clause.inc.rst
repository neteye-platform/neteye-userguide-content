.. _tornado-with-clause:

WITH Clause
```````````

There are multiple ways of configuring the regexes to obtain the desired result while extracting a variable,
and the following entries are common to all configurations:

- **variable name**: For an easier traceability use a meaningful name for your variable, especially if there are multiple variables to extract;
- **from**: The path to the event data to be processed.

The behavior of an extractor is defined by the following three parameters.
Note that all these values are mutually exclusive.

.. _match-extractor:

MATCH Extractor
+++++++++++++++

**MATCH extractor** is used for extracting variables by using an index-based regular expression.

The following fields are available:

- **MATCH**: the index-based regular expression
- **ALL MATCHES**: set it to ``True`` if you want to return all the occurrences matched by the regex and not only the first one.
- **GROUP MATCH IDX**: the index of the group matched by the regex that you want to return. It returns an array with all groups if nothing is specified.

With the Event payload being (here and for other examples below):

.. code:: json

   {
     "text": "Device battery level: 56%"
   }

the extractor should be built as below:

.. figure:: ./../img/carbon/match-extractor.jpg

with the following extracted variable output

.. figure:: ./../img/carbon/match-variable.jpg


Additionally, with ALL MATCHES set to ``False``

.. figure:: ./../img/carbon/all-matches-false.png

the extracted variable output is:

.. figure:: ./../img/carbon/all-matches-false-variable.png


With ALL MATCHES set to ``True``

.. figure:: ./../img/carbon/all-matches-true.png

the extracted variable output is:

.. figure:: ./../img/carbon/all-matches-true-variable.png


.. _single-key-extractor:

SINGLE KEY MATCH Extractor
++++++++++++++++++++++++++

This extractor is used to extract the value from a key:value map by searching the key with a regex.
The field should contain the regex that must match the key of the value that you want to extract.

.. note::

   The regex can only match exactly one key, otherwise an error will be produced.



With the extractor being built as below:

.. figure:: ./../img/carbon/single-key-match.png

expected extracted variable output is

.. figure:: ./../img/carbon/single-key-variable.png

.. _named-match-extractor:

NAMED MATCH extractor
+++++++++++++++++++++

This extractor extracts a variable by using a regex with named groups.

The following fields are available:

- **NAMED MATCH**
- **ALL MATCHES**: set it to ``True`` if you want to return all the occurrences matched by the regex and not only the first one.
  (See examples of an extractor with ALL MATCHES set to ``True``/``False`` in :ref:`MATCH extractor <match-extractor>`).

With the extractor being built as below:

.. figure:: ./../img/carbon/named-match.png

expected extracted variable output is

.. figure:: ./../img/carbon/named-match-variable.png


.. _post-modifiers:

Post Modifiers
++++++++++++++

The WITH clause can include a list of modifiers to post-process the extracted value.

**Lowercase** converts the resulting String to lower case.

**ToNumber** transforms the resulting String into a number.

**Trim** removes the whitespace from the start and end of the string.

**ReplaceAll** replaces all matching substrings with the given text. For this use the following fields:

- Find: the string that you want to replace
- Replace: the string that will replace the Find string
- Regex: if enabled, the Find field will be treated as a regular expression

**Map** map a string to another string value.
It replaces a string with another by looking at a map of Value and Replacement pairs.
The default_value is optional. If the value of the variable doesn't matches a value in the map,
then the Default value is applied

With the Event received being

.. code:: json

  {
    "service_status": "ERROR"
  }

you can set the values and replacements in Mapping as follows

.. figure:: ./../img/carbon/map-example.png

to get the following output

.. figure:: ./../img/carbon/map-output.png


**DateAndTime** converts a timestamp (autodetects if it is in seconds, milliseconds or nanoseconds)
to an RFC3339 standard datetime.
For example the timestamp ``1698933188760``, with the ``Europe/Rome`` timezone, will become ``2023-11-02 14:53:08+01:00`` string.
