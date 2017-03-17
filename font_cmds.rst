.. index:: Font
.. include:: global.rst
Font Controlling Commands
==========================

This section describes all commands that affect how and which font is rendered.

.. raw:: latex

    \newpage

.. _1b21:
.. py:attribute:: Select Print Mode - $1B $21

   Quick-select a variety of print control options such as font type and effects.

   :Format: ``Hex       $1B $21 n`` 

            ``ASCII     ESC !   n``

            ``Decimal   27  33  n``  
   :Range: ``0 ≤ n ≤ 255``
   :Default: ``None``        
   :Notes:
       - See table for appropriate value of n.
       - Underline exceptions
          - Does not underline 90°/270° rotation
          - Does not underline horizontal tabs
          - Underline thickness is specified by ESC - (TODO link)
       - This command resets the left and right margins
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

.. raw:: latex

    \newpage

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

.. raw:: latex

    \newpage

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

.. raw:: latex

    \newpage

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

.. raw:: latex

    \newpage

.. _1b56:
.. py:attribute:: 90° Rotation - $1B $56

   Turns 90° rotation on or off, based on n

       - n = 0, 48 Turns off rotation
       - n = 1, 49 Turns on rotation

   :Format: ``$1B $56 n`` or ``ESC V n`` or ``27 86 n``
   :Range: ``n = 0, 1, 48, 49``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Invalid n values will be ignored
       - Double-width and double-height commands in 90° rotation will enlarge the opposite dimension when this mode is enabled.
         - i.e. width and height scalars are swapped
       - 90° rotated characters will be underlined in the vertical direction.   

   :Related: ``None``
   :Example:
        TODO     

.. raw:: latex

    \newpage

.. _1b7b:
.. py:attribute:: Upside-down Mode - $1B $56

   Turn upside-down print mode on/off

       - When the LSB of n is 0, upside-down print mode is turned off.
       - When the LSB of n is 1, upside-down print mode is turned on.  

   :Format: ``$1B $7B n`` or ``ESC { n`` or ``27 123 n``
   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This command is enabled only when processed at the beginning of the line.
       - When upside-down print mode is turned on, the printer prints all characters rotated 180° from right to left.
       - Upside-down print mode is effective for all data except for the following:
       
          - Graphics from GS TODO ( L <Function 112> or <Function 113>. 
          - Raster bit image from GS v 0 
          - Vairable veritical size bit images GS Q 0 
       - Upside-down print mode is effective until any of the following occur:

          - It is explicitly disabled by settings LSB of n to 0         
          - ESC @ is executed 
          - Printer is reset 
          - Power is turned off 

    .. tip:: The line printing order is not reversed. Therefore, care should be taken when considering the order of the data transmitted.

   :Related: ``None``
   :Example:
        TODO
|upsidedown|

.. raw:: latex

    \newpage      

.. _1bc1:
.. py:attribute:: Set CPI Mode - $1B $C1

   Selects the active CPI mode.

        +------+-----------------------------------------+
        | n    |  CPI Mode                               |
        +======+==================+======================+
        | 0,48 | Font A = 11 cpi  | Font B = 15 cpi      |
        +------+------------------+----------------------+           
        | 1,49 | Font A = 15 cpi  | Font B = 20 cpi      |        
        +------+------------------+----------------------+   
        | 2,50 | Font A = 20 cpi  | Font B = 15 cpi      |        
        +------+------------------+----------------------+   

   :Format: ``$1B $C1 n`` or ``ESC Á n`` or ``27 193 n``
   :Range: ``n= 0, 1, 2, 48, 49, 50``
   :Default: ``n=0, n is base 10``
   :Notes:
       - CPI is characters per inch
       - The higher the CPI, the smaller the font
   :Related: :ref:`Select Print Mode<1b21>`
   :Example:
        TODO        

.. raw:: latex

    \newpage       

.. _1d21:
.. py:attribute:: Select Character Size - $1D $21
    
    Select character width and height according the bits of n.
    - Bits 0 to 3 : select character height (see table 2)
    - Bits 4 to 7 : select character width (see table 1)

    Table 1 - Width
        +-----+---------+---------------+
        | HEX | DECIMAL | Width         |
        +=====+=========+===============+
        | 0   | 0       | 1 (normal)    |
        +-----+---------+---------------+
        | 10  | 16      | 2 (2x width)  |
        +-----+---------+---------------+
        | 20  | 32      | 3 (3x width)  |
        +-----+---------+---------------+
        | 30  | 48      | 4 (4x width)  |
        +-----+---------+---------------+
        | 40  | 64      | 5 (5x width)  |
        +-----+---------+---------------+
        | 50  | 80      | 6 (6x width)  |
        +-----+---------+---------------+
        | 60  | 96      | 7 (7x width)  |
        +-----+---------+---------------+
        | 70  | 112     | 8 (8x width)  |
        +-----+---------+---------------+

    Table 2 - Height
        +-----+---------+---------------+
        | HEX | DECIMAL | Height        |
        +=====+=========+===============+
        | 0   | 0       | 1 (normal)    |
        +-----+---------+---------------+
        | 1   | 1       | 2 (2x height) |
        +-----+---------+---------------+
        | 2   | 2       | 3 (3x height) |
        +-----+---------+---------------+
        | 3   | 3       | 4 (4x height) |
        +-----+---------+---------------+
        | 4   | 4       | 5 (5x height) |
        +-----+---------+---------------+
        | 5   | 5       | 6 (6x height) |
        +-----+---------+---------------+
        | 6   | 6       | 7 (7x height) |
        +-----+---------+---------------+
        | 7   | 7       | 8 (8x height) |
        +-----+---------+---------------+        

   :Format:
        ``Hex       $1B $21  n``  

        ``ASCII     GS   !   n``  
        
        ``Decimal   29  33   n``  

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Invalid n values are ignored, the current character size is maintained.
       - Characters on the same line sized to different heights will be aligned to the topline.
       - Width is expanded to the right.
       - In standard mode, the character is enlarged in the paper feed direction when double-height mode is selected, and it is enlarged perpendicular to the paper feed direction when double-width mode is selected. However, when character orientation changes in 90° clockwise rotation mode, the relationship between double-height and double-width is reversed.
       - :ref:`ESC !<1b21>` Can also be used for this setting. The last received command is the effective one.

   :Related: :ref:`Select Print Mode (ESC !)<1b21>`
   :Example:
        TODO            

.. raw:: latex

    \newpage

.. _1d42:
.. py:attribute:: Reverse Print Mode - $1D $42
    
    Turn white/black reverse printing (inverted) mode on/off based on the LSB of n
    - LSB Set: reverse enabled
    - LSB Clear: reverse disabled

   :Format:
        ``Hex       $1B $42  n``  

        ``ASCII     GS   B   n``  
        
        ``Decimal   29  66   n``  

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Only the LSB of n is inspected.
       - This does not affect images, bardocdes, or user defined images.
       - This has a higher priority than underline. 

          - Underline will stay enabled but no be applied if this setting is enabled.        

   :Related: ``None``
   :Example:
        TODO           