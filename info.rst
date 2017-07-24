.. index:: Printer Information

Printer Information
====================

Commands that provide information about the printer's identity are provided here

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