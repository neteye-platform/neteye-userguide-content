How To Create Independent Pipelines with a Filter
-------------------------------------------------

We can use **Filters** to organize coherent set of **Rules** into
isolated pipelines.

In this example we will see how to create two independent pipelines, one
that receives only events with type 'email', and the other that receives
only those with type 'trapd'.

Our configuration directory will look like this:::

   rules.d
     |- email
     |    |- ruleset
     |    |     |- ... (all rules about emails here)
     |    \- only_email_filter.json
     |- trapd
     |    |- ruleset
     |    |     |- ... (all rules about trapds here)
     |    \- only_trapd_filter.json
     \- filter_all.json

This Processing Tree has a root Filter *filter_all* that matches all
events. We have also defined two inner Filters; the first,
*only_email_filter*, only matches events of type 'email'. The other,
*only_trapd_filter*, matches just events of type 'trap'.

Therefore, with this configuration, the rules defined in *email/ruleset*
receive only email events, while those in *trapd/ruleset* receive only
trapd events.

This configuration can be further simplified by removing the
*filter_all.json* file::

   rules.d
     |- email
     |    |- ruleset
     |    |     |- ... (all rules about emails here)
     |    \- only_email_filter.json
     \- trapd
          |- ruleset
          |     |- ... (all rules about trapds here)
          \- only_trapd_filter.json

In this case, in fact, Tornado will generate an implicit Filter for the
root node and the runtime behavior will not change.

Below is the content of our JSON Filter files.

Content of *filter_all.json* (if provided):

.. code:: json

   {
     "description": "This filter allows every event",
     "active": true
   }

Content of *only_email_filter.json*:

.. code:: json

   {
     "description": "This filter allows events of type 'email'",
     "active": true,
     "filter": {
       "type": "equals",
       "first": "${event.type}",
       "second": "email"
     }
   }

Content of *only_trapd_filter.json*:

.. code:: json

   {
     "description": "This filter allows events of type 'trapd'",
     "active": true,
     "filter": {
       "type": "equals",
       "first": "${event.type}",
       "second": "trapd"
     }
   }
