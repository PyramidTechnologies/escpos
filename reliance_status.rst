.. index:: Reliance Status
.. include:: global.rst


Reliance Status
================
This command provides a wealth of information about the printers current status. The command will always receive a response unless
the printer is offline (powered down, disconnected, etc.) or paper is actively being fed through the printer. A numeric
argument is provided which controls the meaning of each bit in the returned byte(s). There is some duplication between
fields for legacy support reasons but you effectively have access to all error and status conditions.

.. tip:: See realtime status examples on Github: `Thermal Talk API <https://github.com/PyramidTechnologies/ThermalTalk>`_

----------

.. raw:: latex

    \clearpage

.. _r1004:
.. Index:: $10 $04 - Real Time Status

Real Time Status - ``$10 $04`` |rel|
--------------------------------------

   Transmits the printer status in real time.

   :Format:
       ``Hex       $10  $04 n``  

       ``ASCII     DLE  EOT n``  
        
       ``Decimal   16   04   n``      

   :Notes:
       - This command is processed in real time. The reply to this command is sent whenever it is received and does not wait for previous ESC/POS commands to be executed first.

   :Range: ``n = 1 , 2, 3, 4, n = 17, n = 20``

   == =====================================
   n  Status
   == =====================================
   1  Transmit the printer status
   2  Transmit the off-line printer status
   3  Transmit error status
   4  Transmit paper roll sensor status
   17 Transmit the print status
   20 Transmit Full Status (6 Byte Reply)
   == =====================================

   :n=1 (hexadecimal $01) Printer Status:
    
       +-----+--------+------+---------+-------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION |
       +=====+========+======+=========+=============+
       | 0   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 1   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 2   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 3   | Off    | 00   | 0       | Online      |
       |     +--------+------+---------+-------------+
       |     | On     | 08   | 8       | Offline     |
       +-----+--------+------+---------+-------------+
       | 4   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 5   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 6   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+
       | 7   | `-`    | `-`  | `-`     | Reserved    |
       +-----+--------+------+---------+-------------+

   :n=2 (hexadecimal $02) Offline Status:
       
       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | Off    | 00   | 0       | Cover is closed                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 04   | 4       | Cover is open                     |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | Off    | 00   | 0       | Paper is not fed with DIAG button |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 08   | 8       | Paper is fed with DIAG button     |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | Off    | 00   | 0       | Paper is present                  |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 20   | 32      | Printing stopped due to paper end |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | Off    | 00   | 0       | No error                          |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 40   | 64      | Error                             |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       
   .. note:: DIAG Button:
      This bit is *always* set because our diagnostic button is always enabled.
      
   .. note:: Error:
      This bit means that *any* error has been reported. Query the other status commands to determine the precise error.

   :n=3 (hexadecimal $03) Error Status:

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | Off    | 00   | 0       | Cutter Okay                       |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 08   | 8       | Cutter Error                      |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | Off    | 00   | 0       | No unrecoverable error            |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 20   | 32      | Unrecoverable error               |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | Off    | 00   | 0       | No auto-recoverable error         |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 40   | 64      | Auto-recoverable error            |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

   :n=4 (hexadecimal $04) Paper Roll Sensor Status:

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 2,3 | Off    | 00   | 0       | Paper present in abundance        |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 0C   | 12      | Paper low                         |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5,6 | Off    | 00   | 0       | Paper present                     |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 60   | 96      | Paper not present                 |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

   :n=17 (hexadecimal $11) Print Status:

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | Off    | 00   | 0       | Paper motor off                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 04   | 4       | Paper motor on                    |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | Off    | 00   | 0       | Paper present                     |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 20   | 32      | Printing stopped due to paper end |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

   :n=20 (hexadecimal $14) Full Status:

       1st Byte = $10 (DLE)

       2nd Byte = $0F

       3rd Byte

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | Off    | 00   | 0       | Paper Present                     |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 01   | 1       | Paper Not Present                 |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | Off    | 00   | 0       | Paper present in abundance        |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 04   | 4       | Near paper end                    |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | Off    | 00   | 0       | Ticket not present at output      |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 20   | 32      | Ticket present at output          |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

       4th Byte

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | Off    | 00   | 0       | Cover is closed                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 01   | 1       | Cover is open                     |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | Off    | 00   | 0       | Cover is closed                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 02   | 2       | Cover is open                     |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | Off    | 00   | 0       | Paper motor off                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 08   | 8       | Paper motor on                    |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | Off    | 00   | 0       | DIAG button released              |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 20   | 32      | DIAG button pressed               |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

       5th Byte

       +-----+--------+------+---------+-----------------------------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                       |
       +=====+========+======+=========+===================================+
       | 0   | Off    | 00   | 0       | Head temperature ok               |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 01   | 1       | Head temperature ok               |
       +-----+--------+------+---------+-----------------------------------+
       | 1   | Off    | 00   | 0       | No Communication Error            |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 02   | 2       | RS232 Error                       |
       +-----+--------+------+---------+-----------------------------------+
       | 2   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | Off    | 00   | 0       | Power supply voltage ok           |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 08   | 8       | Power supply voltage error        |
       +-----+--------+------+---------+-----------------------------------+
       | 4   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 5   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 6   | Off    | 00   | 0       | Free paper path                   |
       |     +--------+------+---------+-----------------------------------+
       |     | On     | 40   | 64      | Paper jam                         |
       +-----+--------+------+---------+-----------------------------------+
       | 7   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+

       6th Byte

       +-----+--------+------+---------+--------------+
       | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION  |
       +=====+========+======+=========+==============+
       | 0   | Off    | 00   | 0       | Cutter ok    |
       |     +--------+------+---------+--------------+
       |     | On     | 01   | 1       | Cutter error |
       +-----+--------+------+---------+--------------+
       | 1   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 2   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 3   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 4   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 5   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 6   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+
       | 7   | `-`    | `-`  | `-`     | Reserved     |
       +-----+--------+------+---------+--------------+

   :Default: ``None``
   :Related: ``None``

   :Example of No Paper:
    .. code-block:: none

        write("\x10\x04\x04")   # Paper Roll Status
        >>> 0b01101100          # $6C or 108, this means that there is no paper

   :Example of Low Paper:
    .. code-block:: none

        write("\x10\x04\x04")   # Paper Roll Status
        >>> 0b00001100          # $0C or 12, this means that the paper level is low

