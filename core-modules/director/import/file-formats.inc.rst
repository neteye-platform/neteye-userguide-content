.. _fileshipper-file-formats:

Supported File Formats
``````````````````````

Depending on the installed libraries the Import Source currently
supports multiple file formats.

CSV (Comma Separated Value)
+++++++++++++++++++++++++++

`CSV <https://en.wikipedia.org/wiki/Comma-separated_values>`__ is a not
so well defined data format, therefore the Import Source has to make
some assumptions and ask for optional settings.

Basically, the rules to follow are:

-  a header line is required
-  each row has to have as many columns as the header line
-  defining a value enclosure is mandatory, but you do not have to use
   it in your CSV files. So while your import source might be asking for
   ``"hostname";"ip"``, it would also accept ``hostname;ip`` in your
   source files
-  a field delimiter is required, this is mostly comma (``,``) or
   semicolon (``;``). You could also opt for other separators to fit
   your very custom file format containing tabular data

.. rubric:: Sample CSV files


**Simple Example**

.. code::

   "host","address","location"
   "csv1.example.com","127.0.0.1","HQ"
   "csv2.example.com","127.0.0.2","HQ"
   "csv3.example.com","127.0.0.3","HQ"

**More complex but perfectly valid CSV sample**

.. code::

   "hostname","ip address","location"
   csv1,"127.0.0.2","H\"ome"
   "csv2",127.0.0.2,"asdf"
   "csv3","127.0.0.3","Nott"", at Home"

JSON - JavaScript Object Notation
+++++++++++++++++++++++++++++++++

`JSON <https://en.wikipedia.org/wiki/JSON>`__ is a pretty simple
standardized format with good support among most scripting and
programming languages. Nothing special to say here, as it is easy to
validate.

.. rubric:: Sample JSON files


**Simple JSON example**

This example shows an array of objects:

.. code:: json

   [{"host": "json1", "address": "127.0.0.1"},{"host": "json2", "address": "127.0.0.2"}]

This is the easiest machine-readable form of a JSON import file.

**Pretty-formatted extended JSON example**

Single-line JSON files are not very human-friendly, so you’ll often meet
pretty- printed JSON. Such files also make perfectly valid import
candidates:

.. code:: json

   {
     "json1.example.com": {
       "host": "json1.example.com",
       "address": "127.0.0.1",
       "location": "HQ",
       "groups": [ "Linux Servers" ]
     },
     "json2.example.com": {
       "host": "json2.example.com",
       "address": "127.0.0.2",
       "location": "HQ",
       "groups": [ "Windows Servers", "Lab" ]
     }
   }

Microsoft Excel
+++++++++++++++

XSLX, the Microsoft Excel 2007+ format is supported since v1.1.0.

XML - Extensible Markup Language
++++++++++++++++++++++++++++++++

When working with `XML <https://en.wikipedia.org/wiki/XML>`__ please try
to ship simple files as shown in the following example.

.. rubric::  Sample XML file

.. code:: xml

   <?xml version="1.0" encoding="UTF-8" ?>
   <hosts>
     <host>
       <name>xml1</name>
       <address>127.0.0.1</address>
     </host>
     <host>
       <name>xml2</name>
       <address>127.0.0.2</address>
     </host>
   </hosts>

YAML (Ain't Markup Language)
++++++++++++++++++++++++++++

`YAML <https://en.wikipedia.org/wiki/YAML>`__ is anything but simple and
well defined, however it allows you to write the same data in various
ways. This format is useful if you already have files in this format,
but it's not recommended for future use.

.. rubric:: Sample YAML files


**Simple YAML example**

.. code:: yaml

   ---
   - host: "yaml1.example.com"
     address: "127.0.0.1"
     location: "HQ"
   - host: "yaml2.example.com"
     address: "127.0.0.2"
     location: "HQ"
   - host: "yaml3.example.com"
     address: "127.0.0.3"
     location: "HQ"

**Advanced YAML example**

Here's an example using `Puppet <https://puppet.com/>`__ for database
configuration. as an example, but this might work in a similar way for
many other tools.

Instead of a single YAML file, you may need to deal with a directory
full of files. The :ref:`Import Source documentation
<fileshipper-import>` shows you how to configure multiple files. Here
you can see a part of one such file:

.. code:: yaml

   --- !ruby/object:Puppet::Node::Facts
     name: foreman.localdomain
     values:
       architecture: x86_64
       timezone: CEST
       kernel: Linux
       system_uptime: "{\x22seconds\x22=>5415, \x22hours\x22=>1, \x22days\x22=>0, \x22uptime\x22=>\x221:30 hours\x22}"
       domain: localdomain
       virtual: kvm
       is_virtual: "true"
       hardwaremodel: x86_64
       operatingsystem: CentOS
       facterversion: "2.4.6"
       filesystems: xfs
       fqdn: foreman.localdomain
       hardwareisa: x86_64
       hostname: foreman
