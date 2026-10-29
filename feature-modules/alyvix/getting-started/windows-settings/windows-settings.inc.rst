
In the :ref:`previous example <alyvix_example_test_case_windows_start>` we
created an Alyvix ``test case`` consisting of a single test case object.  Most
of the time though, you'll need to chain together multiple test case objects to
synthesize more complex behavior in your apps.  So let's extend our previous
example to run a second action: opening the Windows Settings app.

.. rubric:: 1. Launch

Launch Alyvix Editor with the same file name from our previous example:

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_editor -f start-test

Editor will load the :file:`start-test.alyvix` test case created in the previous example.

.. rubric:: 2. Capture

We want to capture the screen once the Start menu has already opened, so change
the delay number in :nobutton:`Delay [sec] 0` from ``0`` to something higher
like ``5``, then click on :nobutton:`ADD`.

You'll then have 5 seconds to open the Windows Start Menu (just click on the
Start button yourself within that time limit) before Alyvix captures the screen.

.. image:: img/as_delay_five_seconds.png
   :class: image-boxshadow
   :alt: Setting up the 5-second delay before capturing the screen
   :width: 30%

|

.. rubric:: 3. Annotate

After the screen flashes white, you will see the screen grab and the crosshairs
for annotating any selections. *Right click* on the *Settings* icon at the left
side of the Start Menu.  This will *autocontour* the icon.

.. image:: img/ad_settings_selected.png
   :class: image-boxshadow
   :alt: Select Settings after the 5-second screen capture delay
   :width: 50%

|

.. rubric:: 4. Escape

Press :kbd:`Escape` again to return to Editor with the new selection in the
Designer panel at the right.  Assign a *left click* action as in the previous
example by changing the value of the :guilabel:`Action` dropdown from ``None``
to ``Click``:

.. image:: img/ad_settings_click.png
   :class: image-boxshadow
   :alt: Assign the left click action to the Settings icon
   :width: 60%

|

.. rubric:: 5. Rename

In the Selector panel at the bottom left, change the name of our new test case
object from :guilabel:`VisualObject1` to a new one like :guilabel:`PressSettingsIcon`:

.. image:: img/as_rename_second_object.png
   :class: image-boxshadow
   :alt: The settings button test case in Alyvix Selector
   :width: 70%

|

.. rubric:: 6. Script

Using its ``4 arrows icon`` icon seen above, drag the new test case object
into the Scripting panel underneath the existing script element (similarly,
clicking the |alyvix-lineadd-icon| icon will add it to the end of the script):

.. image:: img/ae_second_script_element.png
   :class: image-boxshadow
   :alt: The settings button script element in Alyvix Editor
   :width: 70%

|

.. rubric:: 7. Run

Now call Alyvix Robot by pressing the |alyvix-runblue| button at the top left. You'll
see the Start Menu open, and then a few seconds later a click on the Settings
icon, at which point the Windows Settings app will open.

.. rubric:: 8. Result

At the top of Editor, open the :nobutton:`Console` panel.  This contains the
timing results for each script element.

.. image:: img/ae_console_panel_result.png
   :class: image-boxshadow
   :alt: The results of running the script in Alyvix Editor
   :width: 70%

|

Finally, save and exit Alyvix Editor using the :nobutton:`SAVE` and
:nobutton:`EXIT` buttons at the top.

|
