.. _customize-slm-reports:

How To Create a Customised Theme For SLM Availability Reports
-------------------------------------------------------------

The HTML version of customer's Availability Reports can be modified on a
per-customer basis, allowing therefore to provide reports with a unique,
tailored layout to each customer.

This feature works currently only from the command line; hence a basic
degree of knowledge and acquaintance with the Linux command line and
with HTML and CSS is required in order to successfully complete this
howto.

Custom reports are built using the
`Handlebars <https://handlebarsjs.com/>`__ templating engine; hence all
necessary file that you would use need to have the **.hbs** extension
(like e.g., :file:`custom_logo.hbs`). You can find some basic notions of
Handlebar's syntax and capabilities in the `official language
guide <https://handlebarsjs.com/guide/>`__.

First of all, we mention some important directory that will be used
next:

* Default templates can be found and reused from
  :file:`/usr/share/slmd/templates/`
* Customised templates are stored in a directory under
  :file:`/neteye/shared/slmd/conf/custom_templates/`

**Scenario**

We want to customise a theme for SLM Availability Report
for customer ACME, Inc., which conforms to their corporate identity, by
adding (1) their logo and (2) the font they use in all its reports, that
is, for the *daily*, *weekly*, and *monthly* reports.

**Step #1: Copy the necessary files**

The first step is to create a new directory in which to store the
customised files. We'll call it simply *acme*:

.. code:: bash

   # mkdir -p /neteye/shared/slmd/conf/custom_templates/acme/partials
   # ls -l /neteye/shared/slmd/conf/custom_templates/acme/

.. container:: codeblock

   .. code::

      total 0
      drwxr-xr-x. 2 root root 6 Jun 17 15:16 partials

Next, we need to copy there all the necessary files; we mentioned that
default templates are in ``/usr/share/slmd/templates``. Under that
directory, you find the following files:

.. code::

  ├── partials
  │   ├── report_legend.hbs
  │   ├── report_header.hbs
  │   ├── report_footer.hbs
  │   └── outages_report.hbs
  ├── weekly_report_template.hbs
  ├── monthly_report_template.hbs
  └── daily_report_template.hbs

Files right under the ``templates`` directory define the structure of
the build daily, weekly, and monthly reports and pull the content from
SLM data, while files under the ``templates/partials`` directory contain
only part of the reports and might be included or not in the
above-mentioned three reports.

Since we want to customise the logo in the header, we copy only
:file:`partials/report_header.hbs`:


.. code:: bash

   # cp /usr/share/slmd/templates/partials/report_header.hbs /neteye/shared/slmd/conf/custom_templates/acme/partials

In order to customise a whole report, you should also copy the
appropriate daily, weekly, or monthly template under the ``acme``
directory you just created, for example, to modify only the daily
template, copy the :file:`daily_report_template.hbs`:

.. code:: bash

   # cp /usr/share/slmd/templates/daily_report_template.hbs /neteye/shared/slmd/conf/custom_templates/acme/

If you do not copy any of the daily, weekly, or monthly report, the
:file:`report_header.hbs` file will be used for all templates, which is
indeed what is required in our sample scenario.

.. note:: The *acme* template will now be shown in the drop-down menu
   called **Report Template** that appears when you add a new customer
   under :menuselection:`SLM / Customers`.

**Step #2: Add the logo**

The logo needs to be encoded in *base64*. On Linux you can simply use the
:command:`base64` command, but there are also online converters available:

.. code:: bash

   # base64 logo.png > /neteye/shared/slmd/conf/custom_templates/acme/partials/acme_logo.hbs

We pipe the output to a file, so it is immediately available for
inclusion in the templates.

By using the ``{{~> partials/acme_logo ~}}`` syntax, indeed, Handlebars
will look for a file called **acme_logo** and replace that whole string
with the **verbatim content** of the file, in this case the
:file:`acme_logo.hbs` file saved in the ``partials`` directory.

You can then edit the file report_header.hbs and add the string
``{{~> partials/acme_logo ~}}`` in the most appropriate place of the
template, making sure to omit the suffix:

.. code:: html

   <!-- Partial - SLM Report Header -->

   <!-- SLM Report Name -->
   <section class="section section--evencolumns">
       <img src="data:image/png;base64,{{~> partials/logo~}}" />
       <div class="wpcol wpcol--2of3">
       <h1 class="title title--large">{{report_data.report_name}}</h1>
       </div>
   </section>

Note that the path is **always** relative to the base directory, which
in this case is :file:`/neteye/shared/slmd/conf/custom_templates/acme`.

.. note::
   To get an overview of all available variables used in the template, you
   can look up the files shipped with the template in
   :file:`/usr/share/slmd/templates`.

**Step #3: Customise the CSS**

Since ACME corporate identity uses a given font, ``Lato``, we want its
template to mimic it; therefore we need to change the reports' style to
use that font by default.

Remaining under the ``partials`` directory, we create a new file
**acmestyle.hbs** with the following content:

.. code:: html

   <style>
   body {
       font-family:"Lato","Helvetica New",Arial,sans-serif;
   }
   </style>

As you can see from this snippet, only *inline CSS* is supported in
templates. Finally, we need to tell the header to use the the
custom CSS. Similar to what we did in the previous step for the logo,
we need to make sure that the template includes this file:

.. code:: html

   <!-- Partial - SLM Report Header -->
   {{~> partials/acmestyle ~}}
   <!-- SLM Report Name -->
   <section class="section section--evencolumns">
   {{~> partials/custom_logo ~}}
       <div class="wpcol wpcol--2of3">
       <h1 class="title title--large">{{report_data.report_name}}</h1>
       </div>
   </section>

You can now use your fully customised template for ACME, Inc.'s report
templates: edit the ACME, Inc. customer under *SLM > Customers* and
assign to it the *acme* template.

**Conclusions**

We have shown how to modify a few characteristics of a report template,
but many more possibilities exists: you can completely revamp the layout
of the templates by adding a completely customised CSS; you can add a
background image to the report, modify the fonts used for every
component of the HTML report (``div``, ``span``, ``h2`` etc).

When dealing with Handlebars, like for any other templating engine, it
is nonetheless important to focus on two points:

-  There is no check on the syntax of the file(s) you include, so make
   sure that the elements you include are valid (like, e.g., CSS and
   HTML structure and syntax, correctly encoded images, and so on)

-  In the template, all the strings that need to pull data and content
   from the SLM backend **must** be enclosed in double curly braces
   (like, e.g., ``{{month_day_number}}`` or
   ``{{translate "report.common.week_days.short.mon"}}``
