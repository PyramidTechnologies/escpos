.. index:: Imaging
.. include:: global.rst
Images and Barcode
==================
This section describes functions for raster images, bitmaps, bar codes, and QR Code®.

----

.. raw:: latex

    \clearpage

.. _1c7d25:
.. index:: $1C $7D $25 - 2D Barcode Generator

.. py:attribute:: 2D Barcode Generator - $1C $7D $25 k d1...dk

   Encodes the specified string as a center justified 2D barcode. Only k bytes of the string will be read and any remaining will be treated as regular text or ESC/POS commands. The command and data must be enclosed by :ref:`Line Feeds<x0a>`.

   .. note:: Requires firmware 1.9 or newer

   :Format:
       ``Hex       $1C  $7D 25  k d1...dk``  

       ``ASCII     FS   }   %   k d1...dk``  
        
       ``Decimal   28  125  37  k d1...dk``  
   :Notes:
       - This 2D barcode is compliant with the QR Code® specification and can be read by all 2D barcode readers.
       - This command must be sent when the current line is empty. If not, the command will be ignored, and the bytes to be encoded will be printed as text.
       - Up to 154 8-bit characters are supported.
       - If the input string length exceeds the range specified by the k parameter, only the first 154 characters will be encoded. The rest of the characters to be encoded will be printed as regular ESC/POS characters on a new line.

   :Range: ``0 < k <= 154 8-bit alphanumeric and URL-safe characters``
   :Default: ``None``
   :Related: ``None``
   :Example:
       .. code-block:: none
         :emphasize-lines: 2,3
        
         write("\x0a")           # Beginning line feed
         write("\x1c\x7d\x25")   # Start QR Code® command
         write("\x1C")           # Length of string to follow (28 bytes in this example)
         write("https://pyramidacceptors.com")
         write("\x0a")           # Ending line feed
         print()
         >>> 

QR Code® is a registered trademark of DENSO WAVE INCORPORATED.

|pyramidqr|

----

.. raw:: latex

    \clearpage

.. _1d6b:
.. index:: $1D $6B - Barcode Generator

.. py:attribute:: Barcode Generator - $1D $6B m d1...dk $00

       - Defines and prints a raster bit image using the mode specified by `m`:
       
        +-------+----------------+-------------------+----------------------------+
        | m     | Barcode System | No. of Characters | Valid Characters (decimal) |
        +=======+================+===================+============================+
        | 4     | Code  39       | 1 <= k            | 48 <= d <= 57,             |
        |       |                |                   | 65 <= d =< 90, 32, 36, 37, |
        |       |                |                   | 43, 45, 46, 47, 58         |
        +-------+----------------+-------------------+----------------------------+
        | 8     | Code  128      | 1 <= k            | 1 <= d <= 127              |
        +-------+----------------+-------------------+----------------------------+
        
       - Currently only Code 39 and Code 128 are supported.


   .. note:: Requires firmware 1.18 or newer


   :Format:
       ``Hex       $1D $6B m   d1...dk $00``  

       ``ASCII     GS  k   m   d1...dk NUL``  
        
       ``Decimal   29  107 m   d1...dk 0``  
       
   :Notes:
       - If there is data in the buffer when the printer receives this command, the buffered data will be printed, and the barcode will be printed on the following line.
       - If the barcode generated is too long to be printed, nothing will be printed.
       - If an invalid value of `m` is sent, nothing will be printed.
       - If an invalid character is sent, the text "HRI NOT OK" will be printed.
       - Barcode justification is set by the ``$1B $61`` (Select Justification) command.
       - Barcode height is set by the ``$1D $68`` (Set 1D Barcode Height) command.
       - Barcode width is set by the ``S1D $77`` (Set 1D Barcode Width Multiplier) command.
       
   :Notes for Code 128:
       - Characters that are within the valid range defined in the table above, but are invalid to the current mode are ignored, and not encoded.
       - Special characters (mode select, mode shift, FNC) are transmitted by sending the '{' character before the special character. The first two characters following `m` must select either mode A, B, or C. The '{' character is transmitted by sending two '{' characters. A special character (one that is preceded by '{' but not defined in the table below) are ignored, and not encoded.
       - If the first two characters following `m` do not select a valid mode, the text "HRI NOT OK" is printed.
       
        +===========+=============+=======+=========+
        | Character | Hexadecimal | ASCII | Decimal |
        +===========+=============+=======+=========+
        | Shift     | $7B $53     | { S   | 123 83  |
        +===========+=============+=======+=========+
        | Mode A    | $7B $41     | { A   | 123 65  |
        +===========+=============+=======+=========+
        | Mode B    | $7B $42     | { B   | 123 66  |
        +===========+=============+=======+=========+
        | Mode C    | $7B $43     | { C   | 123 67  |
        +===========+=============+=======+=========+
        | FNC 1     | $7B $31     | { 1   | 123 49  |
        +===========+=============+=======+=========+
        | FNC 2     | $7B $32     | { 2   | 123 50  |
        +===========+=============+=======+=========+
        | FNC 3     | $7B $33     | { 3   | 123 51  |
        +===========+=============+=======+=========+
        | FNC 4     | $7B $34     | { 4   | 123 52  |
        +===========+=============+=======+=========+
        | '{'       | $7B $7B     | { {   | 123 123 |
        +===========+=============+=======+=========+
       

   :Range: See table above for range of valid barcode systems, and the range of valid characters and string lengths for each system.
   :Default: ``None``
   :Related: :ref:`Select Justification<1b61>`
             :ref:`Set 1D Barcode Height<1d68>`
             :ref:`Set 1D Barcode Width Multiplier<1d77>`
   :Example:
        .. code-block:: none
        
            # Encode the text "CODE 39" as a Code 39 barcode
            write("\x1d\x6b\x04\x43\x4f\x44\x45\x20\x33\x39\x00")
            
            # Encode the text "Code 128" as a Code 128 barcode, using mode B
            write("\x1d\x6b\x08\x7b\x42\x43\x6f\x64\x65\x20\x31\x32\x38\x00")


----


.. raw:: latex

    \clearpage

.. _1d77:
.. index:: $1D $77 - Set 1D Barcode Width Multiplier

.. py:attribute:: Set 1D Barcode Width Multiplier - $1D $77 n

   Sets the 1D barcode width multiplier.

   :Format:
       ``Hex       $1D $77 n``  

       ``ASCII     GS  w   n``  
        
       ``Decimal   29  119 n``

   :Notes:
       - The barcode is scaled horizontally by `n` units. A value of 2 doubles the width of each bar in the barcode, doubling the width of the entire barcode. A value of 1 does not scale the barcode. In an unscaled barcode, the thinnest bar has a width of one dot (0.12499975mm, or 0.00492125 inches).
       - This parameter does not need to be set for every barcode. It is only reset to the default value when the printer is rebooted.
       - If an invalid (out of range) value of `n` is sent, the command is ignored.
       - When using code 128, a scalar of 1 may produce barcodes that are valid, but too small to be properly read.
       
   :Range:
       ``1 ≤ n ≤ 6``

   :Default:
       ``n = 2``

   :Example:
        .. code-block:: none
        
            # Set the 1D barcode width to be three times the base width
            write("\x1d\x77\x03")


----


.. raw:: latex

    \clearpage

.. _1d68:
.. index:: $1D $68 - Set 1D Barcode Height

.. py:attribute:: Set 1D Barcode Height - $1D $68 n

   Sets the 1D barcode height, measured in dots.

   :Format:
       ``Hex       $1D $68 n``  

       ``ASCII     GS  h   n``  
        
       ``Decimal   29  104 n``

   :Notes:
       - The barcode height `n` is measured in dots. One dot equals 0.12499975mm, or 0.00492125 inches.
       - This parameter does not need to be set for every barcode. It is only reset to the default value when the printer is rebooted.
       - If an invalid (out of range) value of `n` is sent, the command is ignored.
       
   :Range:
       ``1 ≤ n ≤ 255``

   :Default:
       ``n = 100``

   :Example:
        .. code-block:: none
        
            # Set the 1D barcode height to 50 dots
            write("\x1d\x68\x32")


----

.. raw:: latex

    \clearpage

.. _1d7630:
.. index:: $1D $76 $30 - Raster Image

.. py:attribute:: Raster Image - $1D $76 $30 m xL xH yL yH d1...dk

   Prints a raster image

   :Format:
       ``Hex       $1C  $76 30  m xL xH yL yH d1...dk``  

       ``ASCII     GS   v   %   m xL xH yL yH d1...dk``  
        
       ``Decimal   29  118  48  m xL xH yL yH d1...dk``  
   :Notes:
       - When ​standard mode​ is enabled, this command is only executed when there is no data in the print buffer. (Line is empty) 
       - The defined data (​d​) defines each byte of the raster image. Each bit in every byte defines a pixel. A bit set to 1 is printed and a bit set to 0 is not printed.
       - If a raster bit image exceeds one line, the excess data is not printed. 
       - This command feeds as much paper as is required to print the entire raster bit image, regardless of line spacing defined by :ref:`1/6" or 1/8"<1b32>` commands.
       - After the raster bit image is printed, the print position goes to the beginning of the line. 
       - The following commands have no effect on a raster bit image: 

         - Emphasized  

         - Double Strike 

         - Underline  

         - White/Black Inverse Printing 

         - Upside-Down Printing  

         - Rotation  

         - Left margin 

         - Print Area Width 

       - A raster bit image data is printed in the following order: 

        +--------+--------+--------+--------+
        | d1     | d2     | ...    |   dx   |
        +========+========+========+========+
        | dx + 1 | dx + 2 | ...    | dx * 2 |
        +--------+--------+--------+--------+
        |   .    |   .    |  .     |   .    | 
        +--------+--------+--------+--------+
        |  ...   | dk - 2 | dk - 1 |   dk   |
        +--------+--------+--------+--------+
     

       - Defines and prints a raster bit image using the mode specified by ​m​:
       
        +-------+---------------------+--------------+--------------+
        | m     | Mode                | Width Scalar | Heigh Scalar |
        +=======+=====================+==============+==============+
        | 0, 48 | Normal              | x1           | x1           |
        +-------+---------------------+--------------+--------------+
        | 1, 49 | Double Width        | x2           | x1           |
        +-------+---------------------+--------------+--------------+
        | 2, 50 | Double Height       | x1           | x2           |
        +-------+---------------------+--------------+--------------+
        | 3, 51 | Double Width/Height | x2           | x2           |
        +-------+---------------------+--------------+--------------+
        
        - xL, xH ​defines the raster bit image in the horizontal direction in ​bytes​ using two-byte number definitions. (​xL + (xH * 256)) Bytes
        - yL, yH ​defines the raster bit image in the vertical direction in ​dots​ using two-byte number definitions. (​yL + (yH * 256)) Dots 
        - d ​ specifies the bit image data in raster format. 
        - k ​indicates the number of bytes in the bit image. ​k ​is not transmitted and is there for explanation only.         


   :Range:
       ``0 ≤ m ≤ 3, 48 ≤ m ≤ 51``

       ``1 ≤ xL + (xH * 256) ≤ 65535``

       ``1 ≤ yL + (yH * 256) ≤ 2047``

       ``0 ≤ D1...Dk ≤ 255``

       ``k = (xL + (xH  * 256)) * (yL + (yH * 256))``  

   :Default: ``N/A``
   :Related: ``N/A``
   :Example: `Github <https://github.com/PyramidTechnologies/reliance-helpers>`_

----

.. raw:: latex

    \clearpage

.. _1bfa:  
.. index:: $1B $FA -  Print Graphic Bank/Logo  
.. py:attribute::  Print Graphic Bank/Logo - $1B $FA  

    Prints logo ``n`` from internal storage using dimensions defined as :ref:`Two Byte Numbers<2byte>`.

    :Format: 
             ``Hex      $1B $FA n   xH  xL  yH  yL``

             ``ASCII    ESC {}  n   xH  xL  yH  yL``

             ``Decimal  27  250 n   xH  xL  yH  yL``
    :Notes:
        - n specifies which logo to print.
        - :math:`xL + (xHx256)` specifies the starting dotline.
        - Dolines start at line 0.
        - :math:`yL + (yHx256)` specifies the number of dotlines to print.
        - If :math:`xL + (xHx256)`  is greater than the specified logo's height, the printer does not execute the command.
        - If :math:`[(xL + (xHx256))  + (yL + (yHx256) )]` is greater than the specified logo's height, only dotlines from the specified start dotline to the end of the logo will be printed.
        - If the logo specified by n has not been downloaded or n is out of range, then logo 1 will be printed.

    :Range:
      ``1 ≤ n ≤ 255``

      ``0 ≤ xH, xL, yH, yL, ≤ 255``
    :Default: ``N/A``
    :Related: :ref:`Print Graphic Bank/Logo (Simplified)<1c79>`

    :Example Print logo 1 from dotlines 10 to 200:
        .. code-block:: none

            write('\x1b\xfa\x01\x00\x0a\x00\xc8')

    :Example Print logo 2 from dotlines 0 to 861:
        .. code-block:: none

            write('\x1b\xfa\x02\x00\x00\x03\x5e')   #  862 dotlines total

----

.. raw:: latex

    \clearpage

.. _1c79:  
.. index:: $1C $79 -  Print Graphic Bank/Logo (Simplified)
.. py:attribute::  Print Graphic Bank/Logo (Simplified) - $1C $79  

    Prints logo ``n`` from internal storage using the dimensions stored in flash. This command is similar to the "Print Graphic Bank/Logo" command using the command bytes ``$1B $FA``, but does not need the dimensions to be specified as part of the command.

    :Format: 
             ``Hex      $1C $79 n   $00``

             ``ASCII    FS  y   n   NUL``

             ``Decimal  28  121 n   0``
    :Notes:
        - n specifies which logo to print.
        - The fourth byte of this command is an option specifier reserved for future use. It must be set to zero.
        - If the logo specified by n has not been downloaded or n is out of range, then nothing will be printed.

    :Range:
      ``1 ≤ n ≤ 255``

    :Default: ``N/A``

    :Related: :ref:`Print Graphic Bank/Logo<1bfa>`

    :Example Print the second logo:
        .. code-block:: none

            write('\x1c\x79\x02\x00')

