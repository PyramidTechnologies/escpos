.. index:: Imaging
.. include:: global.rst
Images and Barcode
==================
This section describes functions for raster images, bitmaps, bar codes, and QR Code®.

----

.. 1c7d25:
.. py:attribute:: 2D Barcode Generator - $1C $7D $25 k d1...dk

    Encodes the specified string as a center justified 2D barcode. Only k bytes of the string will be read and any remaining will be treated as regular text or ESC/POS commands. The command and data must be enclosed by line feeds ($0A).

    :Format: ``$1C $7D $25 k D1...Dk``

    :Notes:
        - This 2D barcode is compliant with the QR Code® specicification and can be read by all 2D barcode readers.
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
            write("28")             # Length of string to follow
            write("https://pyramidacceptors.com")
            write("\x0a")           # Ending line feed
            print()
            >>> 

QR Code® is a registered trademark of DENSO WAVE INCORPORATED.

|pyramidqr|

----
