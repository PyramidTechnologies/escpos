.. index:: Phoenix Status
.. include:: global.rst


Phoenix Status
================
This command provides a wealth of information about the printers current status. The command will always receive a response unless
the printer is offline (powered down, disconnected, etc.) or paper is actively being fed through the printer. A numeric
argument is provided which controls the meaning of each bit in the returned byte(s). There is some duplication between
fields for legacy support reasons but you effectively have access to all error and status conditions.

.. tip:: See realtime status examples on Github: `Thermal Talk API <https://github.com/PyramidTechnologies/ThermalTalk>`_

----------

.. raw:: latex

    \clearpage

.. _p1004:
.. Index:: $10 $04 - Real Time Status

Real Time Status - ``$10 $04`` |phx|
-------------------------------------

   Transmits the printer status in real time.

   :Format:
       ``Hex       $10  $04 n``  

       ``ASCII     DLE  EOT n``  
        
       ``Decimal   16   04   n``      

   :Notes:
       - This command will respond with the current real-time status of the printer. It will be processed in order of reception and after any current printing is complete.

   :Range: ``n = 1 , 2, 3, 4``

   == =====================================
   n  Status
   == =====================================
   1  Transmit the printer status
   2  Transmit the off-line printer status
   3  Transmit error status
   4  Transmit paper roll sensor status
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
       | 2   | `-`    | `-`  | `-`     | Reserved                          |
       +-----+--------+------+---------+-----------------------------------+
       | 3   | `-`    | `-`  | `-`     | Reserved                          |
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
      
   .. note:: Error:
      This bit means that *any* error has been reported.

   :n=3 (hexadecimal $03) Error Status:

   .. note:: Error Status:
      This will always return $00 since no errors in Phoenix are auto-recoverable.

   :n=4 (hexadecimal $04) Paper Roll Sensor Status:

       +------+---------+-----------------------------------+
       | HEX  | DECIMAL | DESCRIPTION                       |
       +======+=========+===================================+
       | 1E   | 30      | Paper low                         |
       +------+---------+-----------------------------------+
       | 72   | 114     | Paper not present                 |
       +------+---------+-----------------------------------+


   :Default: ``None``
   :Related: ``None``

   :Example of No Paper:
    .. code-block:: none

        write("\x10\x04\x04")   # Paper Roll Status
        >>> 0b01110010          # $72 or 114, this means that there is no paper

   :Example of Low Paper:
    .. code-block:: none

        write("\x10\x04\x04")   # Paper Roll Status
        >>> 0b00011110          # $1E or 30, this means that the paper level is low

