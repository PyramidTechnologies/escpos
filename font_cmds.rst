.. index:: Font
.. include:: global.rst
Font Controlling Commands
==========================

This section describes all commands that affect how and which font is rendered.

----------


.. _1b2d:
.. py:attribute:: Underline Mode - $1B $2D

   Turns underline mode on or off, based on the following values of n:
       - n = 0, 48 Turns off underline mode
       - n = 1, 49 Turns on underline mode (1-dot thick)
       - n = 2, 50 Turns on underline mode (2-dot thick)

   :Notes:
       - Invalid n values will be ignored. The existing underline and underline thickness settings will be maintained.
       - 90° (ESC V) characters will not be underlined
       - Black/White reverse (GS B) characters will not be underlined.
       - Tab characters are not underlined when this mode is enabled.
       - Disabled or enabling this mode takes is applied immediately. The following data will be underlined.
       - Default underline thickness is 1 dot.
       - Character size does not affect underline thickness.
       - Thickness moves downward from the natural top of the character.
       - ESC ! Can also be used for this setting. The last received command is the effective one.


   :Format: ``$1B $2D n`` or ``ESC - n`` or ``27 45 n``
   :Range: ``0  n ≤ 2, 48 ≤ n ≤ 50``
   :Default: ``0``

   :Example:
        .. code-block:: none

            write("\x1b\x21\x01")          # Enable underline
            write("This is underlined")
            print()
            >>> :underline:`This text is underlined`  # Note that MD format doesn't support underline but trust us :)
            write("\x1b\x21\x00")          # Disable underline
            write("This is not underlined")
            print()
            >>> This is not underlined


