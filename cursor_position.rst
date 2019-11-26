.. index:: Position
.. include:: global.rst

Cursor Position Commands
==========================

This section describes all commands that affect the location of the print position. Consider the print position as a
movable pointer that allows you to print anywhere on the print ticket.

.. raw:: latex

    \clearpage

----

.. _x08:
.. index:: $08 - Backspace 

.. py:attribute:: Backspace - $08

    Moves current print position back 1 character. This command can be used
    to print two or more characters at the same position.

    :Format: 
       ``Hex        $08``

       ``ASCII      BS``

       ``Decimal    8``
    
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

.. raw:: latex

    \clearpage

.. _x09:
.. index:: $09 - Horizontal Tab 

.. py:attribute:: Horizontal Tab - $09

    Advances the horizontal print position to the next column as specified by the Set Horizontal Tab Position command.

    :Format: 
       ``Hex        $09``

       ``ASCII      HT``

       ``Decimal    9``
    
    :Notes:
        - If no tab position has been set, default columns of 8 characters will be used.
        - If this command is received at the end of a line, the current line buffer will be printed and the tab will be
          applied to the next line
        - 1 column width is equal to the width of the current font

    :Range: ``None``
    :Default: ``8 Columns``
    :Related: :ref:`Horizontal Tabs Positions<1b44>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1

            write("Hello\x09World!")    # \x09 is the raw 0x09 character
            print()
            >>> Hello	World?

----

.. raw:: latex

    \clearpage

.. _x0A:
.. index:: $0A - Line Feed

.. py:attribute:: Line Feed - $0A

    Prints the data in the print buffer and feeds one line based on the current line spacing.

    :Format: 
       ``Hex        $0A``

       ``ASCII      LF``

       ``Decimal    10``

    :Notes:
        - Sets the print position to the beginning of the line.

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Carriage Return<x0D>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1,4

            write("Hello World!\x0A")
            print()
            >>> Hello World?
            >>>

----

.. raw:: latex

    \clearpage

.. _x0C:
.. index:: $0C - Form Feed

.. py:attribute:: Form Feed - $0C

    Prints the data in the print buffer, cuts the paper and presents the ticket.

    :Format: 
       ``Hex        $0C``

       ``ASCII      FF``

       ``Decimal    12``

    :Notes:
        - Sets the print position to the beginning of the line.

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Total Cut<1b69>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 4

            write("Hello World!\x0A")
            print()
            >>> Hello World?
            # Paper is cut and presented, buffer is now empty and awaiting more data
            >>>

----

.. raw:: latex

    \clearpage

.. _x0D:
.. index:: $0D - Carriage Return

.. py:attribute:: Carriage Return - $0D

    If CR command is enabled, this command will function exactly like the command $0A does,
    otherwise, the command is ignored.

    :Format: 
       ``Hex        $0D``

       ``ASCII      CR``

       ``Decimal    13``

    :Notes:
        - Sets the print position to the beginning of the line
        - CR can be enabled or disabled with `Reliance Tools <https://pyramidacceptors.com/app/reliance-tools/>`_

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Line Feed<x0A>`

----

.. raw:: latex

    \clearpage

.. _x18:
.. index:: $18 - Cancel Current Line

.. py:attribute:: Cancel Current Line- $18

    Deletes/Cancels the current line

    :Format: 
       ``Hex        $18``

       ``ASCII      CAN``

       ``Decimal    24``

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

.. raw:: latex

    \clearpage

.. _1b24:
.. index:: $1B $24 - Absolute Print Position

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
        - The horizontal and vertical motion units are specified by :ref:`Motion Units <1d50>`. 
          Changing the horizontal or vertical motion unit does not affect the 
          current absolute print position.
        - Absolute print position is effective until it is changed, a new line 
          event occurs, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power 
          is turned off.
        - Even if underline mode is turned on, areas skipped with this command 
          are not underlined.

    :Related: :ref:`Motion Units<1d50>`

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

.. raw:: latex

    \clearpage

.. _1b5c:
.. index:: $1B $5C - Relative Print Position 

.. py:attribute:: Relative Print Position  - $1B $5C

   Relative Print Position

   :Format: 
       ``Hex      $1B $5C  nL nH``

       ``ASCII    ESC \    nL nH``
       
       ``Decimal  27  92   nL nH``

   :Notes:
       - Moves the print position to [(nL + (nH × 256)) × (horizontal or vertical motion unit)] from the current position. Uses :ref:`Two Byte Number Definitions<2byte>`. 
       - A positive number specifies movement to the right, and a negative number specifies movement to the left. 
       - Negative numbers are represented by the complement of 65536. For example, when moving in the left direction N motion units, use: 

         - nL + nH × 256 = 65536-N 

       - Settings that exceed the printable area are ignored. 
       - If settings exceed the print area width, the relative print position is set, but no text will be able to fit in the print area width and any text will be treated as a line feed. 
       - When ​standard mode​ is selected, the horizontal (perpendicular to paper feed) motion unit is used. 
       - When this command is executed, the printer is no longer in a :ref:`New Line State<newlinestate>`
       - The horizontal and vertical motion units are specified :ref:`Motion Units Tab<1d50>` Changing the horizontal or vertical motion unit does not affect the current relative print position. 
       - Even if underline mode is turned on, areas skipped with this command are not underlined. 


   :Range: ``0 ≤ nL, nH ≥ 255, -32768 ≤ (nL + (nH × 256)) ≤ 32767``
   :Default: ``nL = 0, nH = 0``
   :Related: 
      :ref:`Motion Units Tab<1d50>`

      :ref:`Absolute Print Position<1b24>`

   :Example: ``None``
