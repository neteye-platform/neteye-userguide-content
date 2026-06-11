
The Alyvix Designer interface consists of two elements:

* A screen capture image to use for creating and resizing selections and regions of interest
  around visual elements
* A panel for indicating how those visual elements should be interpreted and interacted with

Although Designer is intended to be used in conjunction with Alyvix Editor, you can also run it
as a standalone component from the command line as follows (you can find information about its
:ref:`command arguments here <test_case_building_designer_launch>`):

.. code-block:: doscon
   :class: short-code-block

   C:\Alyvix\testcases> alyvix_designer

When a screen grab begins, the current screen is captured (it will turn white for a second while
the capture process is underway).  It then displays the screen capture at full resolution with
purple crosshairs that track the mouse, and a semi-transparent reminder
:nobutton:`PRESS ESC TO OPEN DIALOG` is overlaid at the top left as shown here:

.. image:: images/ad_main_screen_edit_message_h150.png
   :class: image-boxshadow
   :width: 80%
   :alt: The initial Alyvix Designer selection cursor

.. tip::  Having a second monitor will enable you to mark selections and regions of interest with
   Alyvix on one screen, while the second screen can still be used for other applications. At the
   moment, Alyvix can only grab the screen on the main monitor (*monitor 0*).

Each screen capture is associated with a test case object, which can recognize and interact with
up to three groups of visual elements.  The color of the crosshairs indicates whether you are
working with the first (purple/red), second (green), or third (blue) group.

.. _alyvix_designer_interface_descriptions:

Pressing :kbd:`Escape` will bring up the Designer interface as in the following screenshot, where
no groups (or components) have yet been defined.  The principle interface elements are (the
standalone version of Alyvix Designer is shown here):

.. figure:: /feature-modules/alyvix/editor/images/ad_main_screen_initial_numbered.png
   :figwidth: 50%
   :alt: The empty Alyvix Designer interface


.. rst-class:: bignums

#. The **Object name** (title) of the test case object, which together with the file name is used
   to uniquely identify this test case object in Alyvix Selector and Editor
#. **Test case object** :ref:`options <alyvix_designer_options_test_case_object>`, which affect
   all visual elements in the component tree as a whole; when run from Alyvix Editor, the
   *Timeout* and *Break* options will be shown instead in the
   :ref:`Selector panel <alyvix_selector_interface_screenshot>`
#. The :ref:`component tree subpanel <alyvix_designer_component_tree_top>`, showing all
   defined selections and regions of interest that can be interacted with along with their
   type (image, region or text)
#. :ref:`Component options <alyvix_designer_options_components>`, which depend on the
   type you assign to the currently selected component in the component tree
#. **Interface controls** that allow you to either
   :ref:`continue editing on the screen capture, or exit Designer <alyvix_designer_interface_controls>`.

When Designer is started without any arguments as above, it assigns the default name
:guilabel:`VisualObject1` to the test case object, along with the
:ref:`default options <alyvix_designer_options_test_case_object>`
:guilabel:`Appear`, :guilabel:`Timeout [sec]: 10`, and :guilabel:`Break: Yes`.

When no visual elements have been selected from the screen capture, the component tree will
be empty with only a single root element marked :greyblock:`S`, along with the thumbnail of the
full screen capture.



.. _alyvix_designer_region_bounding:

Selections, Subselections and Regions of Interest
`````````````````````````````````````````````````

To add a new visual component to the tree, you must be in capture mode.  If instead the Designer
or Editor interface is visible, press :wbutton:`EDIT` in the bottom right hand of the panel to
return to the screen capture interface containing the crosshairs.

Selections (and subselections) can be made with the mouse in one of two ways:

* **Manually:**  Hold the left mouse button down to create a selection or subselection (drawing a
  rectangle around the desired area), and then release when done.
* **Autocontour:** Right click on a visual element to use Alyvix's visual recognizer to
  automatically determine the relevant selection or subselection.  Candidate elements can be
  discovered by pressing :kbd:`Space`, and then pressing it a second time to return to the
  standard screen capture.

For instance, you can manually select the Windows Start button using the left mouse button as
shown in the middle image here:

.. figure:: /feature-modules/alyvix/editor/images/ad_screen_capture_combined.png
   :alt: Before and after creating a selection in the screen capture.


After making a selection, you can then make up to 4 subselections within that group, where their
positions will be relative to the main selection.  For instance in the example image above, a
subselection has been made containing the Windows Cortana search box.

Unlike the main selection, a **subselection** consists of two overlapping areas rather than a
single one.  You can resize the two areas independently, although the smaller box, which is
the subselection itself, will always remain contained within the larger one, known as the
**region of interest** (or RoI).

The subselection represents what Alyvix should be looking for, while the region of interest
represents the space in which it should search for that subselection, relative to the main
selection.  The region of interest can be expanded to the edges of the screen if necessary,
which is useful for example when windows or panels can change size dynamically.

.. tip::

   Making the region of interest wider or taller can be very helpful for GUI elements that "float"
   relative to the anchoring selection, such as the window controls when a window is resized.

In order for a group to be considered *matched*, **ALL** selections and subselections (within their
respective regions of interest) must match the screen at the same time.  In turn, in order for a
test case object to be considered *matched*, **ALL** of its groups must match the screen at the
same time.

When creating components, the following keyboard shortcuts are available:

.. table::
   :widths: 30 20 50

   +------------------------------------+-----------------+----------------------------------------------------+
   | **Shortcut**                       | **Focus is on** | **Resulting Action**                               |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`LeftClick(Hold)`             | <Nothing>       | Create a selection or subselection                 |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`LeftClick(Hold)`             | Selection       | Move a selection or subselection somewhere else    |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`Right\ Click`                | <Nothing>       | Autocontour the screen element around the pointer  |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`RightClick`                  | RoI Edges       | Push the edge of the RoI under the mouse all the   |
   |                                    |                 | way to the border of the screen.                   |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`Ctrl` + :kbd:`LeftClick`     | Subselection    | Reset all RoI edges of the subselection to their   |
   |                                    |                 | defaults.                                          |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`Ctrl` + :kbd:`RightClick`    | Subselection    | Remove an entire component (subselection and RoI). |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`Ctrl` + :kbd:`Z`             | Component       | Undo the most recently added component             |
   +------------------------------------+-----------------+----------------------------------------------------+
   | :kbd:`Ctrl` + :kbd:`Y`             | Component       | Re-add the component just removed                  |
   +------------------------------------+-----------------+----------------------------------------------------+

|


.. _alyvix_designer_interface_return_from_sc_mode:

Returning from Screen Capture Mode
``````````````````````````````````

When in screen capture mode, pressing the :kbd:`Escape` key will return you to either the Alyvix
Designer or Editor interface.  As you make new selections and subselections, they will appear as
components within the :ref:`component tree <alyvix_designer_component_tree_top>` as shown here:

.. image:: images/ad_main_screen_new_component2.png
   :class: image-boxshadow zoomable-image
   :width: 50%
   :alt: Adding a first component in the Alyvix Designer interface

Continuing the example from above, the main selection (the Windows Start button) is automatically
set as the group element since it was selected first, while the second area (the Cortana
search box) is set as the first component (subselection) within that group.



.. _alyvix_designer_interface_controls:

Interface Controls
``````````````````

At the bottom of the Designer panel below the component tree are three actions:

* `OK` --- Save the current test case and exit.  If you did not supply a file name when
  you started Designer, it will use the value for :guilabel:`Object name` as the file name.
* `CANCEL` --- Exit Designer without saving the test case.
* `EDIT` --- Return to the screen capture interface.
