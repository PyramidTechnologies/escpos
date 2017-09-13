.. index:: Printer Information

Printer Information
====================

Commands that provide information about the printer's identity are provided here

----

.. raw:: latex

    \clearpage

.. _1d49:  
.. index:: $1D $49 - Printer ID
.. py:attribute:: Printer ID - $1D $49

    Short description

    :Format: 
             ``Hex      $1D $49 n``

             ``ASCII    GS  I   n``

             ``Decimal  29  73  n``
    :Notes:
        - This command responds when the data buffer is processed. Therefore, a time delay between when the command is received and when the printer responds can occur. This time delay depends on the data buffer status and printer status.

        - Refer to this table for valid values of ``n``:

        +------+---------------------+-----------------------------------+
        |  n   | Printer ID          | Description                       |
        +======+=====================+===================================+
        | 1,29 | Printer Model ID    | [Model, Reserved, Reserved]       |
        +------+---------------------+-----------------------------------+
        | 2,50 | Type ID             | RESERVED (Reports $02)            |
        +------+---------------------+-----------------------------------+
        | 3,31 | Firmware Revision   | 4 character revision, e.g. "1.12" |
        +------+---------------------+-----------------------------------+

    :Range: ``1 ≤ n ≤ 3, 49 ≤ n ≤ 51``
    :Default: ``N/A``
    :Related: ``None``
    :Example Model:
        .. code-block:: none
            :emphasize-lines: 2

            write('\x1d\x49\x01')             
            >>> $5D $95 $59             # Reliance model code followed by 2 reserved bytes

    :Example Firmware Revision:
        .. code-block:: none
            :emphasize-lines: 2

            write('\x1d\x49\x03')             
            >>> $31 $2e $31 $32         # 1.12 in ASCII

----

.. _1d72:  
.. index:: $1D $72 - Transmit Status
.. py:attribute:: Transmit Status - $1D $72

    Transmits the paper sensor status based on the value of ``n``. 

    :Format: 
             ``Hex      $1D $72 n``

             ``ASCII    GS  r   n``

             ``Decimal  29  114 n``
    :Notes:
        - This is **not** a real time status command. 
        - Commands will be processed in order of reception, therefore a time delay may be present between receiving the command transmitting the Paper Sensor Status.
        - Refer to this table for response codes:

        +-----------------------------------------------------------------------+
        |     Ejector State Byte Table                                          |
        +-----+--------+------+---------+---------------------------------------+
        | BIT | OFF/ON | HEX  | DECIMAL | DESCRIPTION                           |
        +=====+========+======+=========+=======================================+
        | 0,1 | Off    | 00   | 0       | Paper Roll Present With Abundance     |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 03   | 3       | Near Paper Roll End                   |
        +-----+--------+------+---------+---------------------------------------+
        | 2,3 | Off    | 00   | 0       | Paper Present                         |
        |     +--------+------+---------+---------------------------------------+
        |     | On     | 0C   | 12      | Paper Not Present                     |
        +-----+--------+------+---------+---------------------------------------+
        | 4   |        |      |         | Reserved                              |
        +-----+--------+------+---------+---------------------------------------+
        | 5   |        |      |         | Undefined                             |
        +-----+--------+------+---------+---------------------------------------+
        | 6   |        |      |         | Undefined                             |
        +-----+--------+------+---------+---------------------------------------+
        | 7   |        |      |         | Reserved                              |
        +-----+--------+------+---------+---------------------------------------+

    :Range: ``n=1,49``
    :Default: ``N/A``
    :Related: ``None``

    :Example Paper Status:
        .. code-block:: none
            :emphasize-lines: 2

            write('\x1d\x72\x01')             
            >>> \x03                # Roll is present but near end  