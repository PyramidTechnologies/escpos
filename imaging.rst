.. index:: Imaging
.. include:: global.rst
Images and Barcode
==================
This section describes functions for raster images, bitmaps, bar codes, and QR code.

----

.. 1c7d25:
.. py:attribute:: QR Generator - $1C $7D $25

    Encodes the specified string as a QR code using the specified width.

    :Range: ``None``
    :Default: ``None``
    :Related: ``None``
    :Example:
        .. code-block:: none
            :emphasize-lines: 2,3

            write("\x1c\x7d\x25")	# Start QR command
			write("\x01\xC8")		# 456 dots wide
			write("28")				# Length of string to follow
			write("https://pyramidacceptors.com")            
            print()
            >>> 
			|pyramidqr|

----