.. index:: Movement
.. include:: global.rst


Paper Movement Commands
==========================

This section describes all commands that move the ticket from an idle to print state. This include cuts, ejection,
presenting and retracting.

----------

.. raw:: latex

    \clearpage

.. _1b69:
.. index:: $1B $69 - Partial Cut

Partial Cut - ``$1B $69`` |rel| |phx|
--------------------------------------

   Performs a partial cut on the current ticket.

   :Notes:
       - For Reliance, this command will always execute a Full Cut.
       - If a ticket is not at least the minimum ticket size, then a blank portion will be printed/added to the ticket to make it the minimum size before the cut.
       - If nothing has been printed, then the command is ignored.

   :Format:
       ``Hex       $1B $69``  

       ``ASCII     ESC  i``  

       ``Decimal   27   105``

   :Range: ``None``
   :Default: ``None``
   :Related: :ref:`Form Feed<x0c>`

----------

.. raw:: latex

    \clearpage

.. _1b6D:
.. index:: $1B $6D - Full Cut

Full Cut - ``$1B $6D`` |phx|
-----------------------------

   Performs a full cut on the current ticket.

   :Notes:
       - If a ticket is not at least the minimum ticket size, then a blank portion will be printed/added to the ticket to make it the minimum size before the cut.
       - If nothing has been printed, then the command is ignored.

   :Format:
       ``Hex       $1B $6D``  

       ``ASCII     ESC  m``  

       ``Decimal   27   109``

   :Range: ``None``
   :Default: ``None``
   :Related: :ref:`Form Feed<x0c>`

----------

.. raw:: latex

    \clearpage

.. _1D65:
.. index:: $1D $65 - Ejector

Ejector - ``$1D $65`` |rel|
---------------------------

   Ejector Commands 

   :Notes:
       - The ``​m​`` parameter must be sent for ``n = 3``, and ``n = 32`` . 
       - The ``​t`` parameter must be sent for ``n = 32``.
       - When ``n = 3``, 32 and the value of ``m`` is longer than the current ticket, the ticket will be ejected the length of the ticket. 
       - When ``n = 2, 3, 5, 32``, the printer will cut the ticket before it executes. 
       - When ``n = 32``, and the printer is told to print another ticket, the current ticket will be ejected or retracted based on the printer configuration. When the timeout condition has been met, the ticket is ejected or retracted based on the printer configuration.  
       - When in continuous mode and  ``m = 3, 32``, the ticket is not presented any further if the ticket is at least the minimum ticket size. This command will just enable ticket pull detection and/or the set timeout.
       - This command controls the operation of the ejector and presenter. The command can be used to present, retract and/or produce a blank ticket. Also, this command can enable and disable the ​continuous mode​ feature. The value of ``n​`` determines what the command will do and what additional (if any) parameters it may need. All additional parameters will use ``​m​``  or ``​t``.​ See table below.        

        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | n   | Args                     | Description                                                                                                     |
        +=====+==========================+=================================================================================================================+
        | 1   | None                     | ``None``                                                                                                        |
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 2   | None                     | Retract ticket                                                                                                  |
        |     |                          |                                                                                                                 |
        |     |                          | - Only if paper retracting is enabled                                                                           |
        |     |                          |                                                                                                                 |        
        |     |                          | - This command will cut the ticket if it is not already                                                         |
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 3   | 0 ≤ m ≤ 255              | Present ticket with ``m`` steps                                                                                 |
        |     |                          |                                                                                                                 |
        |     |                          | - 1 step = 7 mm                                                                                                 |
        |     |                          |                                                                                                                 |        
        |     |                          | - This command will cut the ticket if it is not already.                                                        | 
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 5   | None                     | Eject ticket                                                                                                    |
        |     |                          |                                                                                                                 | 
        |     |                          | - This command will cut the ticket if it is not already                                                         |                
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 6   | None                     | Transmit ejector status byte                                                                                    |
        |     |                          |                                                                                                                 |          
        |     |                          | - See :ref:`Ejector Status Table<ejectorstatus>`                                                                |
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 18  | None                     | Disable dispenser continuous mode                                                                               |
        |     |                          |                                                                                                                 |
        |     |                          | - While printing, the ticket remains at printer bezel.                                                          |
        |     |                          |                                                                                                                 |         
        |     |                          |  - The ticket can be cut and presented to the customer                                                          |
        |     |                          |                                                                                                                 |         
        |     |                          |  - The ticket can be cut and retracted back in                                                                  |
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 20  | None                     | Enable dispenser continuous mode                                                                                |
        |     |                          |                                                                                                                 |        
        |     |                          | - While printing, the ticket is continuously pushed from the outlet                                             |
        |     |                          |                                                                                                                 |        
        |     |                          | - This is the default printer state on power up.                                                                |                         
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+
        | 32  | 0 ≤ m ≤ 255              | Present the ticket with m* steps and a timeout *t*                                                              |
        |     |                          |                                                                                                                 |        
        |     | 0 ≤ t ≤ 255              | - 1 step = 7 mm   if it is not already.                                                                         |
        |     |                          |                                                                                                                 | 
        |     |                          | - This command will cut the ticket if it is not already                                                         |               
        +-----+--------------------------+-----------------------------------------------------------------------------------------------------------------+          

        .. _ejectorstatus:
        .. index:: Ejector State Byte Table 

        +-----------------------------------------------------------------------+
        |     Ejector State Byte Table                                          |
        +-----+--------+------+---------+---------------------------------------+
        | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                           |
        +=====+========+======+=========+=======================================+
        | 0   | Off    | 00   | 0       | Paper is present                      |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 01   | 8       | Near paper end                        |
        +-----+--------+------+---------+---------------------------------------+
        | 1   | Off    | 00   | 0       | ``Reserved``                          |
        +-----+--------+------+---------+---------------------------------------+
        | 2   | Off    | 00   | 0       | Paper is not present at printer entry |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 04   | 8       | Paper is present at printer entry     |
        +-----+--------+------+---------+---------------------------------------+
        | 3   | Off    | 00   | 0       | No presented ticket at output         |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 08   | 8       | Presented ticket at output            |
        +-----+--------+------+---------+---------------------------------------+
        | 4   | Off    | 00   | 0       | Printer’s stepper motor is off        |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 10   | 16      | Printer’s stepper motor is on         |
        +-----+--------+------+---------+---------------------------------------+
        | 5   | Off    | 00   | 0       | Printer’s ejector motor is off        |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 20   | 32      | Printer’s ejector motor is on         |
        +-----+--------+------+---------+---------------------------------------+
        | 6   | Off    | 00   | 0       | No error                              |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 40   | 64      | Error                                 |
        +-----+--------+------+---------+---------------------------------------+
        | 7   | Off    | 00   | 0       | Printer has no jam                    |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 80   | 128     | Printer is jammed                     |
        +-----+--------+------+---------+---------------------------------------+

   :Format:
       ``Hex       $1D $65  n   m   t``  

       ``ASCII     GS   E   n   m   t``  

       ``Decimal   29   101 n   m   t``

   :Range: 
     ``1 ≤ n ≤ 3, 5 ≤ n ≤ 6, n = 18, n = 20, n = 32``

     ``0 ≤ m ≤ 255`` 

     ``0 ≤ t ≤ 255``
   :Default: ``N/A``
   :Related: :ref:`Form Feed<x0c>`    
   :Example:
    .. code-block:: none

        write("\x1d\x65\x05")   # Eject Ticket
        write("\x1d\x65\x02")   # Retract Ticket
        
   :Example:
    .. code-block:: none

        write("\x1d\x65\x03\x0c")       # Present 84 mm
        write("\x1d\x65\x20\x0c\x1e")   # Present 84 mm with timeout of 30 seconds 

   :Example:
    .. code-block:: none

        write("\x1d\x65\x14")   # Set continuous mode
        write("\x1d\x65\x12")   # Disable continuous mode

----

.. raw:: latex

    \clearpage

.. _1c7d60:  
.. index:: $1C $7D $60 - Enable and Disable auto cut 

Enable and Disable Auto Cut - ``$1C $7D $60`` |phx|
----------------------------------------------------

    This command changes if the printer will auto-cut a ticket or not.

    :Format: 
             ``Hex      $1C $7D $60  n``

             ``ASCII    FS  }   '    n``

             ``Decimal  28  125  140  n``
    :Notes:

      - Reliance supports enable/disable autocut through the Reliance Tools program.
      - Phoenix supports enable/disable autocut through the Phoenix tools program.
      
            +------+-----------------------------------------+
            |  n   |              Function                   |
            +======+=========================================+
            |  $00 | Disable Auto-Cut, must send cut command |
            +------+-----------------------------------------+
            |  $01 |  Enable Auto-Cut, paper cuts itself     | 
            +------+-----------------------------------------+

      - If a ticket is not at least the minimum ticket size, then a blank portion
        will be printed/added to the ticket to make it the minimum size before 
        the cut.

    :Range: 
      ``n = $00, $01``

    :Default: ``None``
    :Related:
      :ref:`Select Cut Mode and Cut Paper <1d56>`    

    :Example:
    
----

.. raw:: latex

    \clearpage

.. _1d56:  
.. index:: $1D $56 - Select Cut Mode and Cut Paper

Select Cut Mode and Cut Paper - ``$1D $56`` |phx|
----------------------------------------------------

    Select the cut mode for the printer and execute a cut command.

    :Format: 
             ``Hex      $1D $56  n``

             ``ASCII    GS  V   n``

             ``Decimal  29  86  n``
    :Notes:
    
            +------+-----------------------------------------+
            |  n   |              Function                   |
            +======+=========================================+
            |  $00 | Full Cut mode                           |
            +------+-----------------------------------------+
            |  $01 |  Partial Cut mode                       | 
            +------+-----------------------------------------+
      
      - Dip switches overide choice of full or partial



    :Range: 
      ``n = 0,1``

    :Default: ``None``
    :Related: 
        :ref:`Full Cut <1b6d>`
        
        :ref:`Partial Cut <1b69>`    

    :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1b64:  
.. index:: $1B $64 - Print and Feed Paper n Lines 

Print and Feed Paper n Lines - ``$1B $64`` |phx|
----------------------------------------------------

    Print current buffer and feed n number of lines up to a maximum of 200.

    :Format: 
             ``Hex      $1B $64  n``

             ``ASCII    ESC  d   n``

             ``Decimal  27  100  n``
    :Notes:

      - After printing, the print postion is moved to left side of the printable area. Also, the printer is in the status "Beginning of the line".

    :Range: 
      ``n>=0, n<=255``

    :Default: ``None``
    :Related:
      :ref:`Print and Feed Paper <1b4a>`    

    :Example:

----

.. raw:: latex

    \clearpage

.. _1b4a:  
.. index:: $1B $4A - Print and Feed Paper 

Print and Feed Paper - ``$1B $4A`` |phx|
----------------------------------------------------

    Print current buffer and feed paper. Will print in currently selected mode.

    :Format: 
             ``Hex      $1B $4a  n``

             ``ASCII    ESC  j   n``

             ``Decimal  27  74  n``
    :Notes:

      - Any passed n value is ignored.
      - Optional because the Phoenix prints on both (either) CR or LF

    :Range: 
      ``n>=0, n<=255``

    :Default: ``None``
    :Related:
      :ref:`Print and Feed Paper n Lines <1b64>`    

      :ref:`Select Page Mode<1b53>`

      :ref:`Select Standard Mode<1b53>`

    :Example:
