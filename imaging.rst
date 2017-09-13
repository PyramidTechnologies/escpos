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

