.. index:: Imaging
.. include:: global.rst
Images and Barcode
==================
This section describes functions for raster images, bitmaps, bar codes, and QR code.

----

.. 1c7d25:
.. py:attribute:: QR Generator - $1C $7D $25 w d1...dk

    Encodes the specified string as a QR code using the width. Each unit of width is 8 dots which is ~1mm. The resulting QR code will be center justified unless you override the line justification before issuing the command. Only k bytes of the string will be read and any remaining will be treated as regular text or ESC/POS commands.

    :Format: ``$1C $7D $25 w D1...Dk``
    :Range: ``0 < w <= 85`` ``0 < k <= 200 alphanumeric and URL-safe characters``
    :Default: ``None``
    :Related: ``None``
    :Example:
        .. code-block:: none
            :emphasize-lines: 2,3

            write("\x1c\x7d\x25")   # Start QR command
            write("\x25")           # 37 bytes == ~296 mm wide
            write("28")             # Length of string to follow
            write("https://pyramidacceptors.com")            
            print()
            >>> 
|pyramidqr|

----