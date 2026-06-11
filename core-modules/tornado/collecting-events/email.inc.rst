.. _tornado-email-collector:

Email
~~~~~

The *Email Collector* generates Tornado Events from valid `MIME email
messages <https://en.wikipedia.org/wiki/MIME>`__  as inputs.

In NetEye, the incoming email messages are managed by the `Postfix mail server <https://www.postfix.org/documentation.html>`__.
Only the emails sent to the local NetEye user **eventgw** are forwarded to the Tornado Email Collector.
You or your Administrator will need to configure the NetEye Postfix mail server to receive emails for the user **eventgw**.

All the emails received to the before mentioned mailbox are then out of the box forwarded to the
Tornado Email Collector that will parse them and convert in Tornado Events with the extracted data.

To check if the Email Collector is working properly, send an email to the dedicated **eventgw** user
which will then be processed by Tornado:
``# echo "TestContent" | mail -s TestSubject eventgw@localhost``

Now test that an email sent to that address makes it to Tornado
(the timestamp reported by journalctl should be at most a second or two
after you send the email)::

  # journalctl -u tornado_email_collector.service
  Jun 21 15:11:59 host.example.com tornado_email_collector[12240]: [2019-06-21][15:11:59]
  [tornado_common::actors::uds_server][INFO] UdsServerActor - new client connected to [/var/run/tornado/email.sock]``


With the attachments included, the ones that are text files will be
in plain text, otherwise they will be encoded in base64.

For example, passing this email with attachments:

.. code:: mime

   From: "Francesco" <francesco@example.com>
   Subject: Test for Mail Collector - with attachments
   To: "Benjamin" <benjamin@example.com>,
    francesco <francesco@example.com>
   Cc: thomas@example.com, francesco@example.com
   Date: Sun, 02 Oct 2016 07:06:22 -0700 (PDT)
   MIME-Version: 1.0
   Content-Type: multipart/mixed;
    boundary="------------E5401F4DD68F2F7A872C2A83"
   Content-Language: en-US

   This is a multi-part message in MIME format.
   --------------E5401F4DD68F2F7A872C2A83
   Content-Type: text/html; charset=utf-8
   Content-Transfer-Encoding: 7bit

   <html>Test for Mail Collector with attachments</html>

   --------------E5401F4DD68F2F7A872C2A83
   Content-Type: application/pdf;
    name="sample.pdf"
   Content-Transfer-Encoding: base64
   Content-Disposition: attachment;
    filename="sample.pdf"

   JVBERi0xLjMNCiXi48/TDQoNCjEgMCBvYmoNCjw8DQovVHlwZSAvQ2F0YWxvZw0KT0YNCg==

   --------------E5401F4DD68F2F7A872C2A83
   Content-Type: text/plain; charset=UTF-8;
    name="sample.txt"
   Content-Transfer-Encoding: base64
   Content-Disposition: attachment;
    filename="sample.txt"

   dHh0IGZpbGUgY29udGV4dCBmb3IgZW1haWwgY29sbGVjdG9yCjEyMzQ1Njc4OTA5ODc2NTQz
   MjEK
   --------------E5401F4DD68F2F7A872C2A83--

will generate this Event:

.. code:: json

   {
     "type": "email",
     "created_ms": 1554130814854,
     "payload": {
       "date": 1475417182,
       "subject": "Test for Mail Collector - with attachments",
       "to": "\"Benjamin\" <benjamin@example.com>, francesco <francesco@example.com>",
       "from": "\"Francesco\" <francesco@example.com>",
       "cc": "thomas@example.com, francesco@example.com",
       "body": "<html>Test for Mail Collector with attachments</html>",
       "attachments": [
         {
           "filename": "sample.pdf",
           "mime_type": "application/pdf",
           "encoding": "base64",
           "content": "JVBERi0xLjMNCiXi48/TDQoNCjEgMCBvYmoNCjw8DQovVHlwZSAvQ2F0YWxvZw0KT0YNCg=="
         },
         {
           "filename": "sample.txt",
           "mime_type": "text/plain",
           "encoding": "plaintext",
           "content": "txt file context for email Collector\n1234567890987654321\n"
         }
       ]
     }
   }

Within the Tornado Event, the **filename** and **mime_type**
properties of each attachment are the values extracted from the
incoming email.

Instead, the **encoding** property refers to the *content* encoding in
the Event itself, which is one of two types:

-  *plaintext*: The content is included in plain text
-  *base64*: The content is encoded in base64

.. topic:: Special cases

   The email Collector follows these rules to generate the Tornado Event:

   - If more than one body is present in the email or its subparts, the
     first valid body found is used, while the others will be ignored
   - A `Content-Disposition
     <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition>`_
     different from *Inline* and *Attachment* is ignored
   - Content Dispositions of type *Inline* are processed only if the
     content type is *text/\**
   - The email subparts are not scanned recursively, thus only the
     subparts at the root level are evaluated
