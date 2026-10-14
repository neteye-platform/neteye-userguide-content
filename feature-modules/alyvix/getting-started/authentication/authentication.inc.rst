
Often when monitoring applications and services via SaaS, whether in-house or
in the cloud, you'll need to log in to prove you're allowed to use that
particular service. And of course, logging in means that you'll need private
credentials like a user name and password.

When building a test case in Alyvix, where do you store those credentials,
and how are you going to retrieve them when you need them, while keeping
them secure?

Alyvix stores its test cases in an open file format, with the full specification
published in this guide. So if someone manages to obtain your test case files,
they could then look at anything in them, including those credentials. Alyvix
provides a method to encrypt credentials so that they can be stored and
decrypted securely, when needed.

.. todo: :ref:`publish the full specification <test_case_data_format_top>`

Let's start by creating a simple test case that logs in to **www.dropbox.com**
with our credentials and then logs out again, using the best practices described
:ref:`in this previous tutorial <alyvix_example_test_case_web_browser>`.


.. _getting_started_authentication_cipher:

Prepare using Alyvix Cipher
===========================

Before starting the test case build, let's encrypt our credentials so they're
ready to use. We'll make up a private key (remember to keep it securely locked
away, and not on the same computer that the test case runs on), and use it as
one of the arguments to the ``alyvix_cipher`` command.  We'll just have to do
this once for each credential, regardless of how many test cases it's used in.

.. todo: :ref:`alyvix_cipher command <alyvix_robot_cipher_encryption>`:

.. code-block::

   C:\Alyvix\testcases> alyvix_cipher -e <text_string_to_encrypt> -k <private_key>

Suppose we create the private key ``i_like_alyvix`` to encrypt the two
credentials ``my_user_name`` and ``my_password``.  Then we would run the cipher
command twice as follows, producing two ciphers that we'll copy later into the
credential fields:

.. code-block::

   C:\Alyvix\testcases> alyvix_cipher -e my_user_name -k i_like_alyvix
   +HGl7XV2mdTFvWLpr9CtVQ==
   C:\Alyvix\testcases> alyvix_cipher -e my_password -k i_like_alyvix
   bS1boKctW64yZ1dG1iz2ZA==

The cipher command is deterministic, so if you give it the same credential and
same private key, it will produce the same cipher.  When it's time to build the
relevant test case step, just use the cipher returned by the command above as
if that value were the real credential, and Alyvix will decrypt it
automatically when the test case runs, inserting the original value.

Note that the ``==`` at the end of the cipher is what tells Alyvix that it
**is** a cipher.


.. _getting_started_authentication_build_start:

Initial Build Steps
===================

Now that our ciphers are ready, let's start building our test case:

.. rubric:: 1. From a blank desktop

Repeat steps #1-4 :ref:`in the previous tutorial <alyvix_example_test_case_web_browser>`
to launch the web browser, but using ``www.dropbox.com`` instead of
``www.visittrentino.it`` as the web app address. Once the page loads you should
see this at the top:

.. image:: img/gs_authentication_login.png
   :class: image-boxshadow
   :width: 80%
   :alt: The top of the initial web page for www.dropbox.com

|

.. rubric:: 2. The login button

Now let's add a new step that clicks on the login button once the web page has
loaded. Highlight an anchor (an image near the top left of the page like the
Dropbox logo that you know will always be in the same location) and the Login
button, then add a Click action to the Login button:

.. image:: img/gs_authentication_login_editor.png
   :class: image-boxshadow
   :width: 80%
   :alt: Highlighting the anchor and Login button in the web page

|

.. image:: img/gs_authentication_login_click.png
   :class: image-boxshadow
   :width: 60%
   :alt: The click action added to the Login button

|

.. rubric:: 3. The login page

When that step runs, it will take us to the login page where we can enter our
user name:

.. image:: img/gs_authentication_field1.png
   :class: image-boxshadow
   :width: 80%
   :alt: The account field highlighted in a test case step

Then we show Alyvix how to click on the account field, type the user name
cipher we created above into the :guilabel:`String` field, and click on the
Continue button.

.. image:: img/gs_authentication_login_username.png
   :class: image-boxshadow
   :width: 65%
   :alt: The click actions and user name insertion added in Alyvix Designer

|

.. rubric:: 4. Entering the password

When that runs, a second page appears where we can enter the password, and we
do the same thing with the password cipher in the that String field:

.. image:: img/gs_authentication_field2.png
   :class: image-boxshadow
   :width: 80%
   :alt: The password field highlighted in a test case step

|

.. image:: img/gs_authentication_login_password.png
   :class: image-boxshadow
   :width: 60%
   :alt: The click actions and password insertion added in Alyvix Designer

|

.. rubric:: 5. Measure and log out

You can now run whatever task you like within the web app (here, Dropbox) and
measure the results as we did in the previous example test case. For instance,
you can measure how much time it takes for the landing page to appear,
effectively measuring how long the login process took.

When the task you're monitoring is done, you'll need to log out so that the
next time you run the test case, it will show the login page rather than launch
the main app at the landing page without ever asking for a login.

The typical logout procedure at this point is to click to open the Account
menu, and then a second click to actually log out. That's two whole steps.
But we can actually do this in one step if we note that all that the logout
button does is call the URL ``https://www.dropbox.com/logout``:

.. image:: img/gs_authentication_logout.png
   :class: image-boxshadow
   :width: 99%
   :alt: The shortcut for logging out in multi-level windowing

So instead, let's just add a step to insert that URL directly into your
browser's URL bar. This step should locate the bar, click on it, and then have
Alyvix type in the string ``https://www.dropbox.com/logout{enter}`` into the
bar. The ``{enter}`` part tells Alyvix to
:ref:`press the Enter key <alyvix_designer_options_strings_special>` after it
has typed in that string, which will cause the browser to load that URL
(curly braces indicate a special key to Alyvix).

That logs you out, so finally you can kill the browser as described in Step #6
:ref:`in the previous tutorial <alyvix_example_test_case_web_browser>` and
everything will be left in the same state that we started with, allowing you to
repeatedly run the test case for monitoring scenarios.

You can now follow the procedure above to create and insert ciphers no
matter how many encrypted strings you need, and you won't have to worry that
someone will steal your stored credentials.

|
