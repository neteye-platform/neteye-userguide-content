
This short task will accomplish something simple, yet will show you the potential of
using Alyvix.

You can find similar tutorials in video format.

.. todo:  :ref:`video format <video_tutorials_top>`.

.. rubric:: 1. Launch Alyvix Editor

Give this example a file name (the exact name is not important, but we will reuse
it in subsequent examples):

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_editor -f start-test

The Editor screen will appear:

.. image:: img/ae_empty_panel.png
   :class: image-boxshadow
   :alt: The Windows Start button in Alyvix Designer
   :width: 99%

|

.. rubric:: 2. Start the screen capture

Click on the ``DELAY [SEC]  0  ADD`` button at the bottom. This will trigger a
screen capture, which we can then annotate using the mouse. We'll use this
screen grab over the next few steps to build a concrete action that Alyvix will
perform.

.. todo: :wbutton:

.. rubric:: 3. Create a *selection* around the Windows Start button

Use the mouse to *left click* and *drag* at the bottom left of the screen grab
so that it looks similar to this screenshot (if you don't get it quite right
the first time, just resize any of the edges using the left mouse button):

.. image:: img/gs_screen_capture_start_button.png
   :class: image-boxshadow
   :alt: Selecting the Windows Start button.
   :width: 70%

|

.. rubric:: 4. Press the :kbd:`Escape` key

The :kbd:`Escape` key will return us to the Editor interface where the area we
selected containing the Start button is now shown in the Designer panel at the
right. We've just used the screen grab to create a *test case object*, and now
we can begin to change its parameters.

.. image:: img/ae_basic_designer_panel.png
   :class: image-boxshadow
   :alt: The Windows Start button in Alyvix Editor's Designer panel
   :width: 60%

|

.. rubric:: 5. Find the :guilabel:`Action` dropdown

At the bottom of the panel you'll find a dropdown option with the value
``None``. Change that value to ``Click`` and keep its default values. A mouse
icon to the right of the Start button will appear, confirming this with a red
highlight where the left mouse button is.

.. image:: img/ae_designer_panel_selection.png
   :class: image-boxshadow
   :alt: The click point added to the Windows Start button in the Designer panel
   :width: 60%

|

.. rubric:: 6. Save the new test case

Click the ``SAVE`` button at the top of the screen to save this new
*test case*. Since we started Editor with the option ``-f start-test``, the
resulting file will be saved as :file:`start-test.alyvix` in the directory
we were in at Step 1.

.. todo: nobutton
.. todo: :ref:`the resulting file <test_case_data_format_top>`

.. rubric:: 7. Change the file name

Go to the Selector panel at the bottom left and change the name of our test case
object from the default name :guilabel:`VisualObject1` to a more meaningful one
like :guilabel:`PressStartButton`:

.. image:: img/as_rename_object.png
   :class: image-boxshadow
   :alt: The start button test case in Alyvix Selector
   :width: 70%

|

.. rubric:: 8. Script the action

Add our new action to the replay script by clicking on the ``lineadd-icon``
action to the left of the label :guilabel:`PressStartButton`.

.. todo: |lineadd-icon|

Insert the new test case object into the Scripting panel under the blue tab
marked ``Script: MAIN``:

.. todo: :bbutton:

.. image:: img/ae_basic_script_element.png
   :class: image-boxshadow
   :alt: The start button script element in Alyvix Editor
   :width: 70%

|

.. rubric:: 9.  Run the test case

Finally, start Alyvix Robot by pressing the ``RUN`` button at the top left.

.. todo:  |runblue|

The Editor window will disappear and the mouse will move over to the Windows
Start button and click, causing the Start menu to open.

If you saw the Start menu appear, then you've successfully built and run your
very first Alyvix test case!

Finally, save and exit Alyvix Editor using the ``SAVE`` and ``EXIT`` buttons
at the top.

.. todo: :nobutton:

The following page will continue this example to create a more complex script.

|
