.. index:: Position
.. include:: global.rst

Cursor Position Commands
==========================

This section describes all commands that affect the location of the print position. Consider the print position as a
movable pointer that allow you to print anywhere on the print ticket.

----

.. _x08:
.. py:attribute:: Backspace - $08

    Moves current print position back 1 character. This command can be used
    to print two or more characters at the same position.

    :Range: ``None``
    :Default: ``None``
    :Related: ``None``
    :Example:
        .. code-block:: none
            :emphasize-lines: 2,3

            write("Hello World!")
            write('\x08')           # Send tab
            write("?")              # Write a new character
            print()
            >>> Hello World?

----

.. _x09:
.. py:attribute:: Horizontal Tab - $09

    Advances the horizontal print position to the next column as specified by the Set Horizontal Tab Position command.

    :Format: ``$09`` or ``HT`` or ``9``
    :Notes:
        - If no tab position has been set, default columns of 8 characters will be used.
        - If this command is received at the end of a line, the current line buffer will be printed and the tab will be
          applied to the next line
        - 1 column width is equal to the width of the current font

    :Range: ``None``
    :Default: ``8 Columns``
    :Related: :ref:`Horizontal Tabs Positions <1b44>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1

            write("Hello\x09World!")    # \x09 is the raw 0x09 character
            print()
            >>> Hello	World?

----

.. _x0A:
.. py:attribute:: Line Feed - $0A

    Prints the data in the print buffer and feeds one line based on the current line spacing.

    :Format: ``$0A`` or ``LF`` or ``10``
    :Notes:
        - Sets the print position to the beginning of the line.

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Carriage Return <x0D>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1,4

            write("Hello World!\x0A")
            print()
            >>> Hello World?
            >>>

----

.. _x0C:
.. py:attribute:: Form Feed - $0C

    Prints the data in the print buffer, cuts the paper and presents the ticket.

    :Format: ``$0C`` or ``FF`` or ``12``
    :Notes:
        - Sets the print position to the beginning of the line.

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Total Cut <1b69>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 4

            write("Hello World!\x0A")
            print()
            >>> Hello World?
            # Paper is cut and presented, buffer is now empty and awaiting more data
            >>>

----

.. _x0D:
.. py:attribute:: Carriage Return - $0D

    If CR command is enabled, this command will function exactly like the command $0A does,
    otherwise the command is ignored.

    :Format: ``$0D`` or ``CR`` or ``13``
    :Notes:
        - Sets the print position to the beginning of the line
        - CR can be enabled or disabled with `Reliance Tools <https://pyramidacceptors.com/app/reliance-tools/>`_

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Line Feed <x0A>`

----

.. _x18:
.. py:attribute:: Cancel Current Line- $18

    Deletes/Cancels the current line

    :Format: ``$18`` or ``CAN`` or ``24``

    :Notes:
        - Sets the print position to the beginning of the line

    :Range: ``None``
    :Example:
        .. code-block:: none
            :emphasize-lines: 2,3

            write('Hello World')              # Put some text in the buffer
            write('\x18')                     # Send cancel command
            write('Thank you!')               # Write some other data
            write('\x1b\x69')                 # Force-print the buffer
            print()
            >>>Thank you! 

----

.. _1b24:
.. py:attribute:: Absolute Print Position - $1B $24

    Moves the print position to [(nL + (nH × 256)) × (horizontal or vertical 
    motion unit)] from the left edge of the print area. Uses Two Byte Number 
    Definitions. See :ref:`Terminology Section <terminology>`

    |absolutepp|

    :Format: 
             ``Hex      $1B $24 nL  nH``

             ``ASCII    ESC $   nL  nH``

             ``Decimal  27  36  nL  nH``
    :Range: ``None``
    :Default: ``nL = 0, nH = 0``
    :Notes:
        - Settings that exceed the printable area are ignored.
        - If settings exceed the print area width, the absolute print position 
          is set, but no text will be able to fit in the print area width and 
          any text will be treated as a line feed.
        - When standard mode is selected, the horizontal 
          (perpendicular to paper feed) motion unit is used.
        - When this command is executed, the printer is no longer in a 
          “New Line” state. See :ref:`Terminology Section <terminology>`
        - The horizontal and vertical motion units are specified by ``$1D $50``. 
          Changing the horizontal or vertical motion unit does not affect the 
          current absolute print position.
        - Absolute print position is effective until it is changed, a new line 
          event occurs, ``ESC @`` is executed, the printer is reset, or the power 
          is turned off.
        - Even if underline mode is turned on, areas skipped with this command 
          are not underlined.

    :Related: :ref:`Motion Units <1d50>`

              :ref:`Relative Print Position<1b5c>`
              
              .. @todo $1D $D0 command
    :Example:
        .. code-block:: none
            :emphasize-lines: 2

            write('\x1d\x57\x2c\x01')   # Set print area width of 300
            write('\x1b\x24\x64\x00')   # Set absolute print position to 100
            write('\x1b\x4d\x01')       # Select character font 
            # Write text to see multiple lines
            write('Print area width of 300 and absolute print position of 100.
             Only the first line should have this absolute print position.')
            print()
            >>>     
                   Print area wid
            th of 300 and absolut 
            e print position of 1
            00. Only the first li 
            ne should have this a 
            bsolute print positio 
            n.

----

.. _1b44:
.. py:attribute:: Horizontal Tab Positions - $1B $44

    Sets the horizontal tab positions calculated from the start of the line.

    :Format: 
             ``Hex      $1B $44  n1...nk 00``

             ``ASCII    ESC D    n1...nk 00``

             ``Decimal  27  68   n1...nk 00``
    :Notes:
        - N is the column number specified from line start
        - K is the total number of horizontal tab positions
        - Sending zero or NUL terminates the command arguments
        - The final position is n+1, e.g. a position set to 10 when called from the start of a line, will advance the position to 11.
        - Up to 32 columns may be defined. Anything in excess of 32 will be treated as regular data.
        - N1...nk must be sent in ascending order and be terminated with a 0 NUL code.
        - If this format is violated, the data will be treated as regular data.
        - This command cancels previous settings

    :Range: ``None``
    :Default: ``9, 17, 25, 33, 41 ...``
    :Related: :ref:`Horizontal Tab <x09>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1

            write('\x1b\x44\x00')             # Cancel previous tab settings, restores defaults
            write('\x1b\x44\x08\x14\x25\x00') # Set tab stops at 8, 20, and 32 characters
            write('Item\x09Quantity\x09Price')
            print()
            >>> Item		Quantity	Price

---

.. _1b5c:
.. index:: $1B $5C

.. py:attribute:: Relative Print Position  - $1B $5C

    Relative Print Position 