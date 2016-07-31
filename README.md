### QDSpy

This is a software for generating and presenting stimuli for visual neuroscience. It is written in Python and based on  (http://wvad.mpimf-heidelberg.mpg.de/abteilungen/biomedizinischeOptik/software/qds/), uses OpenGL and primarly targets Windows 7 (and above).

*Note, this is an early beta version.*

See documentation in ``/QDSpy/html/index.html`` or http://qdspy.eulerlab.de/.

To get started, see ``/QDSpy/html/installation.html``.

----------------------------------------------
Known issues
----------------------------------------------

* The rotation parameter for videos rotates around a corner, not the center.

* Shader-enabled objects may not be drawn in the correct order, which means
  that shader objects can flicker when they overlap.

* Starting a movie for the first time in a script can cause a delay of ~200 ms.
  A work-around is to start the movie right at the beginning of the script
  starting section, for example:
  ::
    QDS.StartScript()
    QDS.Start_Movie(1, (0,0), [ 0,  0, 1,  1], (1,1), 0, 0)
    QDS.Scene_Clear(2.00, 0)

  This avoids the delay later during the time-critical parts of a stimulus. 

----------------------------------------------
To do
----------------------------------------------

* URGENT: Check dictionaries that are written by the user to history and log
  file for large lists or data structures; these should go into separate files
  to prevent delaying the stimulus presentation 
* Check timing of digital I/O signals (i.e. triggers).
* Complete lightcrafter interface.
* For color modes >1 (special lightcrafter modes), movie images need to be
  converted with the respective cvolor scheme.
* Add possibitity to control timing (i.e. sync refresh to an external source) 
  on genlock-enabled NVIDIA and/or ATI graphic cards.

----------------------------------------------
Release notes
----------------------------------------------

v0.71 beta (July 2016) 

* **Videos (AVI containers) now work** except for the rotation parameter, which uses
  a corner instead of the centre. The commands are `DefObj_Video()`, `Start_Video()` 
  and `GetVideoParamters()`. Note that the latter now returns a dictionary instead
  of a list.
* Like `GetVideoParamters()`, `GetMovieParamters()` now also returns a dictionary.
* The GUI has been reorganized:

  * A font is used that works on all Window versions (7 and higher).
  * *Lightcrafter only*: The `LEDs: enable` button allows switching off the LEDs
    and changes to manual LED control (for details, see section :doc:`lightcrafter`).
  * A new tab called "tools" was added; currently in contains a very simple 
    interface to a USB camera (e.g. for observing the stimulus).
  * A "Wait for trigger: ..." button has been added; *this is not yet functional*.
  * Duration of stimulus is shown.
  * Minor fixes.


v0.70 beta (July 2016) 

* Now reports GLSL version
* Fixed error when QDSpy GUI does not find a compiled `__autorun`.
* Stimulus folder can now be selected using the  `Change folder` button. Note
  that at program start, QDSpy always pre-selects the default stimulus folder
  defined in the `QDSpy.ini` file
* Cleaning up some pyglet-related code
* Fixed that `psutil` was required even with `bool_incr_process_prior=False`
* Added two entries to the `[Display]` section of the configuration file: 
  `bool_markershowonscreen` determines if the marker ("trigger") that is send
  via an I/O card is indicated as a small box in the right-bottom corner of
  the screen. Entry `int_markerrgba` (e.g. = 255,127,127,255) defines the 
  marker color as RGB+alpha values.
* Starting the GUI version does not require a command line parameter anymore
* Added a new optional command line parameter ``--gui``, which when present
  directs all messages (except for messages generated when scripts are 
  compiled) to the history window of the GUI. 
* If the `QDSpy.ini` file is missing, it will be recreated from default values.
  From now on, the `QDSpy.ini` file will not be any longer in this repository to 
  prevent that new versions overwrite setup-specific settings (e.g. hardware 
  use, digital I/O configuration, etc.)
* Removed currently unused tab "Setting" in GUI
* Changed code for increasing process priority via `psutil` 
* Added pre-compiled ``hidapi`` packages for Python 3.5.x (``hid.cp35-win_amd64.pyd``); 
  the version for Python 3.4.x was renamed to ``hid.cp34-win_amd64.pyd``. 
* **Now QDSpy fully supports Python 3.5.x.** The installation instructions were 
  updated accordingly.
* Fixed bug in drawing routines for sectors and arcs
* 'Q' now aborts stimulus presentation for both the command line and GUI 
  version of QDSpy
* Refractoring of the code that interfaces with the graphics API (currently
  OpenGL via pyglet)
* Fixed a bug in the way the scene is centered and scaled.
* Added a **stimulus control window** to be displayed on the GUI screen when 
  stimulus presentation is running fullscreen on a different display. This is beta;
  the performance of drawing stimuli on different screens in parallel needs to
  be carefully tested on different machines.
  Added two entries to the `[Display]` section of the configuration file: 
  `bool_use_control_window` activates or deactivates the control window, and 
  `float_control_window_scale` is a scaling factor that defines the size of the
  control window.
* When upgrading a QDSpy installation, new configuration file parameters are now
  automatically added. 
  **Important:** for consistency, the names of some entries in the configuration 
  file needed to be changed: `str_digitalio_port_out`, `str_digitalio_port_in`,
  `str_led_filter_peak_wavelengths`, `str_led_names`, `str_led_qtcolor`, and 
  `str_markerrgba`. **This needs to be corrected manually in any pre-existing
  configuration file in an existing installation.** Alternatively, the configuration
  file can be deleted, triggering QDSpy to generated a new one with default values.  
* Fixed a bug that caused QDSpy to freeze when changing stage parameters and
  then playing a stimulus.  
* End program cleanly and with a clear error message when digital I/O is enabled
  in the configuration file but the Universal Library drivers are not installed. 
  The default for the configuration file is now `bool_use_digitalio = False`.


v0.6 beta (April 2016) 

* Bug fixes
* Added `DefObj_Video()`, `Start_Video()` and `GetVideoParamters()` commands. 
  Not yet fully implemented and tested; do not use yet.
* Added `GetMovieParameters()` command
* Fixed `GetDefaultRefreshRate()` command; now reports requested frame rate.
* Added `[Tweaking]` section to configuration file. Here, parameters that tune
  the behaviour of QDSpy are collected. The first parameter is `bool_use3DTextures`;
  it determines, which of pyglets texture implementations is used to load and
  manage montage images for movie objects (0=texture grid, 1=3D texture).
* Added instructions to the documentation of `DefObj_Movie()` how to convert
  a movie image montage used for the previous QDS with QDSpy. Key is that - in 
  contrast to QDS - QDSpy considers the bottom-left frame of a montage the first
  frame of the sequence.


v0.5 beta (December 2015) 

* Bug fixes
* Documentation updated
* Gamma correction of display now possible (see section 
  :doc:`how_QDSpy_works`)
* Now catches errors generated when compiling shader scripts.
* Changed stage parameters (magification, center, rotation, etc.) are now 
  written to the configuration file.
* Writes the log automatically to a file. The log contains machine-readable 
  tags for the data analysis.


v0.4 alpha (November 2015) 

* Migrated to Python 3.4.3
* Added GUI
* Bug fixes
* Added rudimentary support for controlling a lightcrafter device 
* Added movie commands, currently only copying what the old QDS commands did.
* Added a shader that acknowledges colour and alpha value of its object 
  ("SINE_WAVE_GRATING_MIX").


v0.3 alpha (March 2015) 

* Minor bug fixes
* Fixed transparency of objects (works now)

v0.2 alpha (before 2015)

* Basic functionality, proof of concept

