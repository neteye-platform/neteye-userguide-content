SLM allows users to customize the appearance of the Availability Report by
overwriting the templates for the report. The default templates are stored in
:file:`/usr/share/slmd/templates/`. Custom reports are built using the
`Handlebars <https://handlebarsjs.com/>`__ templating engine; hence all
necessary files that you would use need to have the **.hbs** extension. You can
find some basic notions of Handlebar's syntax and capabilities in the `official
language guide <https://handlebarsjs.com/guide/>`__. Together with the templates,
we also provide a list of all related variables in the same file. There are
different types of variables:

* **boolean**: A boolean, that can be evaluated in an :code:`{{#if }}`
* **integer**: A normal integer.
* **float**: A floating-point number. Should not be currently used because of
  formatting issues.
* **string**: A string.
* **list<type>**: A list with values of a specific type. It can be iterated
  over with :code:`{{#each }}` to get access to the inner fields.
* **optionals**: These types are listed in the file as :code:`null|*type*`.
  Variables of this type will evaluate to :code:`false` in a
  :code:`{{#if }}` block, if they are null.

For a step-by-step guide check out :ref:`customize-slm-reports`

.. note::
  If a custom template is set, then future updates to the base templates
  will not be visible as it is not used in favor of the custom one.
  Therefore the changes made might need to be migrated in the future.


.. _slmd_advanced_topics_customizing_templates_styling:

Styling
```````

To add styles to the report, you can use the :code:`<style>` tag in the
template. However, with the introduction of the Content-Security-Policy (CSP)
header, all style elements must include the icingaweb2 style nonce to be
compliant with the CSP header and applied properly. The style nonce can be
added to the style element like this: :code:`<style nonce="{{ style_nonce }}">`.
The style nonce will then be passed on each request by icingaweb2 and inserted
by the templating engine.


Helper Functions
````````````````

Helper functions are functions that can be called from the template to do a
certain task. There are currently these helper-functions that can be used in
the templates:

* **translate**: The translate helper-function takes a key as an input and
  depending on the set *LOCALE* outputs the correspondent translation, saved
  in :file:`/usr/share/slmd/i18n/`

* **markdown**: The markdown helper-function takes a string as an argument and
  interprets it as `GitHub Flavored Markdown <https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax>`__,
  outputting HTML. For this, the function must be allowed to write raw HTML to
  the template. This can be achieved with triple curly brackets:
  :code:`{{{markdown }}}`. Should the input string have contained any html than
  it will be sanitized by the function.

* **round**: The round function takes a mode, an integer and a float as
  arguments, in the exact same order as defined above.
  The modes available are: *ceil*, *floor*, *round* and *banker*.
  It will then round the float according to the selected mode and
  the integer precision specific.

Security
````````

Customizing templates gives you the ability to allow for unescaped HTML to be
embedded into the report with the triple curly brackets while the normal double
brackets sanitize the input. This can lead to openings for Cross Site Scripting
(XSS) attacks, if an attacker gains access to the input over an other channels.
Therefore, only the fields with the double brackets or with the
:code:`markdown` helper-function, which escapes the input, should be included.
