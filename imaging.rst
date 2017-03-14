.. index:: Imaging
.. include:: global.rst
Images and Barcode
==================
This section describes functions for raster images, bitmaps, bar codes, and QR code.

----

.. 1c7d25:
.. py:attribute:: QR Generator - $1C $7D $25 k d1...dk

    Encodes the specified string as a QR code. The resulting QR code will be center justified. Only k bytes of the string will be read and any remaining will be treated as regular text or ESC/POS commands. The command and data must be enclosed by line feeds ($0A).

    :Format: ``$0A $1C $7D $25 k D1...Dk $0A``
    :Range: ``0 < k <= 150 alphanumeric and URL-safe characters``
    :Default: ``None``
    :Related: ``None``
    :Example:
        .. code-block:: none
            :emphasize-lines: 2,3

            write("\x0a")           # Beginning line feed
            write("\x1c\x7d\x25")   # Start QR command
            write("28")             # Length of string to follow
            write("https://pyramidacceptors.com")
            write("\x0a")           # Ending line feed
            print()
            >>> 
|pyramidqr|

----
