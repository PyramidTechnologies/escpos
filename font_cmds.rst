.. index:: Font
.. include:: global.rst
Font Controlling Commands
==========================

This section describes all commands that affect how and which font is rendered.

.. raw:: latex

    \clearpage

----

.. _1b40:
.. index:: $1B $40 - Initialize 

.. py:attribute:: Initialize - $1B $40
   
   Clears the data in the print buffer and resets the printer modes to the modes that were in effect when the power was turned on. 
   
   :Format: ``Hex       $1B $40`` 

            ``ASCII     ESC @``

            ``Decimal   27  64`` 
   :Range: ``None``
   :Default: ``None``   
   :Notes:
       - Print buffer is cleared 
       - Data buffer contents are preserved
       - NV graphics (NV bit image) information is maintained. 
       - User NV memory data is maintained. 

   :Related: ``None``
   :Example: ``None``   

----

.. raw:: latex

    \clearpage

.. _1b21:
.. index:: $1B $21 - Select Print Mode 

.. py:attribute:: Select Print Mode - $1B $21

   Quick-select a variety of print control options such as font type and effects.

   :Format: ``Hex       $1B $21 n`` 

            ``ASCII     ESC !   n``

            ``Decimal   27  33  n``  
   :Range: ``0 ≤ n ≤ 255``
   :Default: ``None``        
   :Notes:
       - See table for the appropriate value of n.
       - The baseline for characters of different vertical scalars will be the same

       Underline exceptions
          - Does not underline 90°/270° rotation
          - Does not underline horizontal tabs
          - Underline thickness is specified by :ref:`Underline Mode<1b2d>`
       This command resets the left and right margins
          - Left margin set by :ref:`Left Margin<1d4c>`
          - Right margin set by :ref:`Print Area Width<1d57>`
       For each of the underline, italic, bold modes:
          - These can be issued by their respective ESC commands or this command
          - The last received command is the effective command.

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

   :Related: ``None``
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

----

.. raw:: latex

    \clearpage

.. _1b2d:
.. index:: $1B $2D - Underline Mode 

.. py:attribute:: Underline Mode - $1B $2D

   Turns underline mode on or off, based on the following values of n:

       - n = 0, 48 Turns off underline mode
       - n = 1, 49 Turns on underline mode (1-dot thick)
       - n = 2, 50 Turns on underline mode (2-dot thick)

   :Format: ``Hex       $1B $2D n`` 

            ``ASCII     ESC -   n``

            ``Decimal   27  45  n``  

   :Range: ``0  n ≤ 2, 48 ≤ n ≤ 50``
   :Default: ``0``
   :Notes:
       - Invalid n values will be ignored. The existing underline and underline thickness settings will be maintained.
       - :ref:`90° Rotation<1b56>` characters will not be underlined
       - :ref:`Black/White Reverse<1d42>` characters will not be underlined.
       - Tab characters are not underlined when this mode is enabled.
       - Disabled or enabling this mode takes is applied immediately. The following data will be underlined.
       - Default underline thickness is 1 dot.
       - Character size does not affect underline thickness.
       - Thickness moves downward from the natural top of the character.
       - :ref:`Select Print Mode<1b21>` Can also be used for this setting. The last received command is the effective one.

   :Related: ``None``
   :Example:
        .. code-block:: none

            write("\x1b\x2d\x01")          # Enable underline
            write("This is underlined")
            print()
            >>> __This text is underlined__
            write("\x1b\x2d\x00")          # Disable underline
            write("This is not underlined")
            print()
            >>> This is not underlined

----

.. raw:: latex

    \clearpage

.. _1b34:
.. index:: $1B $34 - Italics Mode 

.. py:attribute:: Italics Mode - $1B $34

   Turns *italics* mode on or off, based on the following values of n:

       - n = 0, 48 Turns off *italics* mode
       - n = 1, 49 Turns on *italics* mode

   :Format: ``Hex       $1B $34 n`` 

            ``ASCII     ESC 4   n``

            ``Decimal   27  52  n``  

   :Range: ``0 ≤ n ≤ 1, 48 ≤ n ≤ 59``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This effect is applied immediately
       - :ref:`Select Print Mode<1b21>` can also be used for this settings. The last received command is the effective one.

   :Related: ``None``
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

----

.. raw:: latex

    \clearpage

.. _1b45:
.. index:: $1B $45 - Emphasis Mode 

.. py:attribute:: Emphasis Mode - $1B $45

   Turns **emphasis** mode on or off, based on the LSB of n:

       - n & 1  = 0 Turns off **emphasis** mode
       - n & 1  = 1 Turns on **emphasis** mode

   :Format: ``Hex       $1B $45 n`` 

            ``ASCII     ESC E   n``

            ``Decimal   27  69  n``  

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This effect is applied immediately
       - Only the LSB of n is inspected
       - :ref:`Select Print Mode<1b21>` can also be used for this settings. The last received command is the effective one.

   :Related: ``None``
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

----

.. raw:: latex

    \clearpage

.. _1b4D:
.. index:: $1B $4D - Select Character Font 

.. py:attribute:: Select Character Font - $1B $4D

   Selects character font based on n.

        +-----+------+----------------------+
        | Set | n    | DESCRIPTION          |
        +=====+======+======================+
        | A   | 0,48 | Select Font A        |
        +-----+------+----------------------+
        | B   | 1,48 | Select Font B        |          
        +-----+------+----------------------+         

   :Format: ``Hex       $1B $4D n`` 

            ``ASCII     ESC M   n``

            ``Decimal   27  77  n`` 

   :Range: ``n = 0, 1, 48, 49``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Up to two fonts can be active at one time

   :Related: ``None``   
   :Example: ``None``             

----

.. raw:: latex

    \clearpage

.. _1b56:
.. index:: $1B $56 - 90° Rotation

.. py:attribute:: 90° Rotation - $1B $56

   Turns 90° rotation on or off, based on n

       - n = 0, 48 Turns off rotation
       - n = 1, 49 Turns on rotation

   :Format: ``Hex       $1B $56 n`` 

            ``ASCII     ESC V   n``

            ``Decimal   27  86  n`` 

   :Range: ``n = 0, 1, 48, 49``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Invalid n values will be ignored
       - :ref:`90° Rotation<1b56>` characters will be underlined in the vertical direction.     

   .. tip:: :ref:`Double-width<1b21>` and :ref:`Double-height<1b21>` commands in :ref:`90° Rotation<1b56>` will enlarge the opposite dimension when this mode is enabled. i.e. width and height scalars are swapped

   :Related: ``None``
   :Example: ``None``   

----

.. raw:: html

    </div>
    <div id="1b74">

.. _1b74:
.. index:: $1B $74 - Select Character Code Page

.. py:attribute:: Select Character Code Page - $1B $74
    
    Select character code page based on ``n``

   :Format:
        ``Hex       $1B $74  n``  

        ``ASCII     ESC t   n``  
        
        ``Decimal   27  116 n``  

   :Range: ``See n Table Below``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Reserved values of ``n`` should not be used in your application     
       - Names with an asterisk (*) may require updated firmware

       +------+----------------------------------------------------+
       |  n   |  Font Code Page                                    |
       +======+====================================================+
       | 0    | **Default**                                        |
       |      |                                                    |
       |      | USA ASCII + Cyrillic                               |
       +------+----------------------------------------------------+
       | 2    | Reserved                                           |
       +------+----------------------------------------------------+
       | 3    | CP437 (Spanish)                                    |
       +------+----------------------------------------------------+       
       | 4    | Reserved                                           |
       +------+----------------------------------------------------+
       | 5    | Reserved                                           |
       +------+----------------------------------------------------+
       | 17   | CP808 (Cyrillic)                                   |
       +------+----------------------------------------------------+
       | 18   | Georgian Mkhedruli*                                |
       +------+----------------------------------------------------+       
       | 19   | Reserved                                           |                     
       +------+----------------------------------------------------+
       | 255  | Reserved                                           |       
       +------+----------------------------------------------------+

   :Related: ``None``
   :Example: ``None``   

----

.. raw:: latex

    \clearpage

.. _1b7b:
.. index:: $1B $7B - Upside-down Mode

.. py:attribute:: Upside-down Mode - $1B $7B

   Turn upside-down print mode on/off

       - When the LSB of n is 0, upside-down print mode is turned off.
       - When the LSB of n is 1, upside-down print mode is turned on.  

   :Format: ``Hex       $1B $7B n`` 

            ``ASCII     ESC {   n``

            ``Decimal   27  123  n`` 

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - This command is enabled only when processed at the beginning of the line.
       - When upside-down print mode is turned on, the printer prints all characters rotated 180° from right to left.
       - Upside-down print mode is effective for all data except for the following:
       
          - Raster bit image from :ref:`Raster Image<1d7630>`

       - Upside-down print mode is effective until any of the following occur:

          - It is explicitly disabled by settings LSB of n to 0         
          - :ref:`Initialize<1b40>` is executed 
          - Printer is reset 
          - Power is turned off 

   .. tip:: The line printing order is not reversed. Therefore, care should be taken when considering the order of the data transmitted.

   :Related: ``None``
   :Example: |upsidedown|

---- 

.. raw:: latex

    \clearpage

.. _1bc1:
.. index:: $1B $C1 - Set CPI Mode

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

   :Format: ``Hex       $1B $C1 n`` 

            ``ASCII     ESC Á   n``

            ``Decimal   27  193  n`` 

   :Range: ``n= 0, 1, 2, 48, 49, 50``
   :Default: ``n=0, n is base 10``
   :Notes:
       - CPI is characters per inch
       - The higher the CPI, the smaller the font
   :Related: :ref:`Select Print Mode<1b21>`
   :Example: ``None``        

----


.. raw:: latex

    \clearpage

.. _1c7d26:
.. index:: $1C $7D $26 - Select Codepage

.. py:attribute:: Select Codepage - $1C $7D $26
    
    Used to select any installed codepage as the active codepage. Using Two Byte Number Definitions, send the integer number
    of the codepage. For example, if codepage 437 is desired, then send the integer 437.

   :Format:
        ``Hex       $1C $7D $26  xL xH``  

        ``ASCII     FS    }   &  xL xH``  
        
        ``Decimal   28  125  36  xL xH``  

   :Range: ``0 ≤ xL + (xH * 256) ≤ 65535``

   :Default: Default codepage is set with PC tools
   :Notes:
       - Codepage = (xL + (xH * 256))
       - If the codepage sent to the printer is not installed, the currently active codepage will not change. 
       - See :ref:`Two Byte Numbers<2byte>` section for more information on two byte number definitions.      

   :Related: :ref:`Select Codepage<1b74>`
   :Example Select Codapge 437:
        .. code-block:: none

            write('\x1c\x7d\x26\xb5\x01')   #  Select codepage 437

   :Example Select Codapge 1252:
        .. code-block:: none

            write('\x1c\x7d\x26\xe4\x04')   #  Select codepage 1252
        


----


.. raw:: latex

    \clearpage

.. _1d21:
.. index:: $1D $21 - Select Character Size

.. py:attribute:: Select Character Size - $1D $21
    
    Select character width and height according to the bits of n.
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
        ``Hex       $1D $21  n``  

        ``ASCII     GS   !   n``  
        
        ``Decimal   29  33   n``  

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Invalid n values are ignored, the current character size is maintained.
       - Characters on the same line sized to different heights will be aligned to the topline.
       - Width is expanded to the right.
       - In standard mode, the character is enlarged in the paper feed direction when double-height mode is selected, and it is enlarged perpendicular to the paper feed direction when double-width mode is selected. However, when character orientation changes in 90° clockwise rotation mode, the relationship between double-height and double-width is reversed.
       - :ref:`Select Print Mode<1b21>` Can also be used for this setting. The last received command is the effective one.

   :Related: :ref:`Select Print Mode<1b21>`
   :Example: ``None``            

----

.. raw:: latex

    \clearpage

.. _1d42:
.. index:: $1D $42 - Reverse Print Mode

.. py:attribute:: Reverse Print Mode - $1D $42
    
    Turn white/black reverse printing (inverted) mode on/off based on the LSB of n
    - LSB Set: reverse enabled
    - LSB Clear: reverse disabled

   :Format:
        ``Hex       $1D $42  n``  

        ``ASCII     GS   B   n``  
        
        ``Decimal   29  66   n``  

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``n=0, n is base 10``
   :Notes:
       - Only the LSB of n is inspected.
       - This does not affect images, barcodes, or user defined images.
       - This has a higher priority than underline. 

          - Underline will stay enabled but no be applied if this setting is enabled.        

   :Related: ``None``
   :Example: ``None`` 

----

