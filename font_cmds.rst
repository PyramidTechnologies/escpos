.. index:: Font
.. include:: global.rst
Font Controlling Commands
==========================

This section describes all commands that affect how and which font is rendered.

----------

.. _1b21:
.. py:attribute:: Select Print Mode - $1B $21

   Quick-select a variety of print control options such as font type and effects.

   :Format: ``$1B $21 n`` or ``ESC ! n`` or ``27 33 n``  
   :Range: ``0 ≤ n ≤ 255``
   :Default: ``None``        
   :Notes:
       - See table for appropriate value of ``n``
       - Underline exceptions
         - Does not underline 90°/270° rotation
         - Does not underline horizontal tabs
         - Underline thickness is specified by ESC - (TODO link)
       - This command reste left and right margins
         - Left margin set by GS L (TODO link)
         - Right margin ste by GW W (TODO link)
       - For each of the underline, italic, bold modes:
         - These can be issues by their respective ESC commands or this command
         - The last received command is the effective command.
       - The basline for characters of different vertical scalars will be the same

        +-----+----------+------+---------+------------------------------+
        | BIT | State    | HEX  | DECIMAL | Function                     |
        +=====+==========+======+=========+==============================+
        | 0   | Disabled | 00   | 0       | Select character font A      |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 01   | 1       | Select character font B      |
        +-----+----------+------+---------+------------------------------+
        | 1   | --       | --   | --      | Reserved                     |
        +-----+----------+------+---------+------------------------------+
        | 2   | --       | --   |  --     | Reserved                     |
        +-----+----------+------+---------+------------------------------+
        | 3   | Disabled | 00   | 0       | Disable emphasis (bold) mode |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 08   | 8       | Enabled emphasis (bold) mode |
        +-----+----------+------+---------+------------------------------+
        | 4   | Disabled | 00   | 0       | Disable double-height mode   |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 10   | 16      | Enable double-height mode    |
        +-----+----------+------+---------+------------------------------+
        | 5   | Disabled | 00   | 0       | Disable double-width mode    |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 20   | 32      | Enable double-width mode     |
        +-----+----------+------+---------+------------------------------+
        | 6   | Disabled | 00   | 0       | Disable italic mode          |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 40   | 64      | Enable italic mode           |
        +-----+----------+------+---------+------------------------------+
        | 7   | Disabled | 00   | 0       | Disable underline mode       |
        |     +----------+------+---------+------------------------------+
        |     | Enabled  | 80   | 128     | Enable underline mode        |
        +-----+----------+------+---------+------------------------------+          

   :Related: ``:ref:`TODO```
   :Example:
        .. code-block:: none

            write("\x1b\x21\x01")          # Select Font B
            write("\x1b\x21\x10")          # Select double-height mode            
            write("This is font B, double-height")
            print()
            >>> This is font B, double-height
            write("\x1b\x21\x00")          # Select Font A and disable double-height
            write("This is font A")
            print()
            >>> This is font A"

.. _1b2d:
.. py:attribute:: Underline Mode - $1B $2D

   Turns underline mode on or off, based on the following values of n:
       - n = 0, 48 Turns off underline mode
       - n = 1, 49 Turns on underline mode (1-dot thick)
       - n = 2, 50 Turns on underline mode (2-dot thick)

   :Format: ``$1B $2D n`` or ``ESC - n`` or ``27 45 n``
   :Range: ``0  n ≤ 2, 48 ≤ n ≤ 50``
   :Default: ``0``
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

   :Related: ``:ref:`TODO```
   :Example:
        .. code-block:: none

            write("\x1b\x2d\x01")          # Enable underline
            write("This is underlined")
            print()
            >>> This text is underlined  # Note that MD format doesn't support underline but trust us :)
            write("\x1b\x2d\x00")          # Disable underline
            write("This is not underlined")
            print()
            >>> This is not underlined

.. _1b34:
.. py:attribute:: Italics Mode - $1B $34

   Turns *italics* mode on or off, based on the following values of n:
       - n = 0, 48 Turns off *italics* mode
       - n = 1, 49 Turns on *italics* mode

   :Format: ``$1B $34 n`` or ``ESC 4 n`` or ``27 52 n``
   :Range: ``0 ≤ n ≤ 1, 48 ≤ n ≤ 59``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This effect is applied immediately
       - ESC! can also be used for this settings. The last received command is the effective one.

   :Related: ``:ref:`TODO```
   :Example:
        .. code-block:: none

            write("\x1b\x34\x01")              # Enable italics
            write("This is italic")
            print()
            >>> This is italic
            write("\x1b\x34\x00")              # Disable italics
            write("This is not italic")
            print()
            >>> This is not italic       

.. _1b45:
.. py:attribute:: Emphasis Mode - $1B $45

   Turns **emphasis** mode on or off, based on the LSB of n:
       - n & 1  = 0 Turns off **emphasis** mode
       - n & 1  = 1 Turns on **emphasis** mode

   :Format: ``$1B $45 n`` or ``ESC E n`` or ``27 69 n``
   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This effect is applied immediately
       - Only the LSB of n is inspected
       - ESC! can also be used for this settings. The last received command is the effective one.

   :Related: ``:ref:`TODO```
   :Example:
        .. code-block:: none

            write("\x1b\x45\x01")              # Enable emphasis
            write("This is bold")
            print()
            >>> This is bold
            write("\x1b\x45\x00")              # Disable itemphasisalics
            write("This is not bold")
            print()
            >>> This is not bold      

.. _1b4D:
.. py:attribute:: Select Character Font - $1B $4D

   Selects character font based on n.

        +----------------------+------+----------------------+
        | Chars/Inch ($1B $C1) | n    | DESCRIPTION          |
        +======================+======+======================+
        | A = 11 cpi           | 0,48 | 11 cpi font selected |
        |                      +------+----------------------+
        | B = 15 cpi           | 1,49 | 15 cpi font selected |  
        +----------------------+------+----------------------+
        | A = 15 cpi           | 0,48 | 15 cpi font selected |
        |                      +------+----------------------+
        | B = 20 cpi           | 1,48 | 20 cpi font selected |  
        +----------------------+------+----------------------+        
        | A = 20 cpi           | 0,48 | 20 cpi font selected |
        |                      +------+----------------------+
        | B = 15 cpi           | 1,48 | 15 cpi font selected |          
        +----------------------+------+----------------------+   

   :Format: ``$1B $4D n`` or ``ESC M n`` or ``27 77 n``
   :Range: ``n = 0, 1, 48, 49``
   :Default: ``n=0, n is base 10``
   :Notes:
       - CPI means characters per inch
       - A higher CPI equates to a smaller, more compact font

   :Related: ``:ref:`TODO```   
   :Example:
        .. code-block:: none

            TODO                 

