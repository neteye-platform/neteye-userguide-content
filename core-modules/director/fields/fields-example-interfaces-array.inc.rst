
Managing Fields
```````````````

This example wants to show you how to make use of the ``Array`` data
type when creating fields for custom variables. First, please got to the
``Dashboard`` and choose the ``Define data fields`` dashlet:

.. figure::  /core-modules/director/img/define-datafields.png
   :alt: Dashboard - Define data fields

   Dashboard - Define data fields

Then create a new data field and select ``Array`` as its data type:

.. figure::  /core-modules/director/img/add-datafield.png
   :alt: Define data field - Array

   Define data field - Array

Then create a new ``Host template`` (or use an existing one):

.. figure::  /core-modules/director/img/add-host-template.png
   :alt: Define host template

   Define host template

Now add your formerly created data field to your template:

.. figure::  /core-modules/director/img/add-template-field.png
   :alt: Add field to template

   Add field to template

That’s it, now you are ready to create your first corresponding host.
Once you add your formerly created template, a new form field for your
custom variable will show up:

.. figure::  /core-modules/director/img/create-host.png
   :alt: Create host with given field

   Create host with given field

Have a look at the config preview, it will show you how your
``Array``-based custom variable will look like once deployed:

.. figure::  /core-modules/director/img/config-preview.png
   :alt: Host config preview with Array

   Host config preview with Array
