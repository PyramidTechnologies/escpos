.. index:: Introduction

Command Table Layout
====================

.. raw:: latex

    \clearpage

Each command will be described in the format as shown below.

:Name:
   The name of the command

:Format:
    Hex, ASCII and  Decimal values of the command

:Range:
   Range of acceptable values for a command and its parameters

:Default:
   A description of the command and what settings it affects

:Notes:
   Other notes about the command and how it operates

:Related:
   Lists any command that relates to the command

:Example:
   Displays and example of the command if it is available


Commands are represented in hexadecimal, ascii, and decimal, in that order. We use the following notation to
indicate what form is being used.

.. _formatOrder:  
.. index:: Format Order 

.. py:attribute:: Format Order

    :Format: 
        ``$Hex 1st``  

        ``ASCII 2nd``
        
        ``Dec 3rd``

----

.. index:: Terminology

.. _terminology:

This section describes the terminology used in this document to describe the command and their functions.

Data Buffer
   Buffer for storing ESC/POS commands

Print Buffer
   Buffer for storing ticket image before printing

New Line
   A new line (line empty state) is a state that satisfies the following conditions:

   1. There is currently no print data in the buffer

   #. There is no skipped portion using the HT (horizontal tab) command

   #. A print position has not been specified using :ref:`Absolute<1b24>` or :ref:`Relative<1b5c>` print position commands
   
   
.. index:: Dots and Pixels   

Dots vs Pixels vs Bytes
    In the context of Reliance printing, we prefer to speak in terms of dots. A dot is a literal
    dot on the thermal printer burn head. This is essentialyl the same concept as a pixel on your monitor.
    Each of our dots are 0.12499975mm (0.00492125") in diameter. This means we achieve a dot-density of 203.2, 
    or more commonly stated as 203 DPI. Some of our commands are simplified to use bytes, or clusters of 8 dots.
    This reduces errors due to incorrect Two-byte calculations and provides slightly faster command parsing. 

    A good rule of thumb is that 1 byte == 8 dots == 1mm. Keep this in mind when you are printing images and dealing with
    margins. Another import number is 680. That is the absolute maximum number of dots we support on our absolute widest
    paper roll (80mm). 


.. _2byte:

Two-byte Number Definitions

   Many ESC/POS commands use two-byte number definitions to represent large numbers in
   two data bytes. In order to represent numbers greater than 255 in this way, we perform
   and integer division and a modulo division to obtain the high and low bytes, respectively.

   The common terms, nH and nL are used throughout this document and refer to the high and low
   bytes, respectively.

   .. note:: ProTip:
      If the target value is less than 256, set nH to 0 and nL to the
      target value.


   :Example 1:

      To load the value 456 into two bytes, you first must solve for the quotient.

      :math:`nH = Quotient = \frac{456}{256} = 1`

      Then solve for the modulo, where the value `b` is the result from above.

      :math:`nL = Modulo = 456-(256*(b)) = 200`

      The resulting byte order transmitted to the printer would then be [01, 200] where transmission is from left to right.

   :Example 2:

      The :ref:`Left Margin<1d4c>` command requires a two-byte parameter for horizontal motion units. To get a left margin of 549 motion units

      :math:`nH = Quotient = \frac{549}{256} = 2`

      :math:`nL = Modulo = 549 - (2 * 256) = 37`

      :math:`\therefore nH = 2 and nL = 37`

      :math:`Verify: 37 + (2 * 256) = 549`

   :Example 3:

      To represent a negative number, use the identity

      :math:`(nL + (nH * 256)) = 65536 - (value)`.

      If we needed to represent the value -324 we would do the following:

      :math:`65536 - 324 = 65212`

      :math:`nH = Quotient = \frac{65212}{256}=254`

      :math:`nL = Modulo = 65212 - (254 * 256) =188`

      :math:`\therefore nH = 254 and nL = 188`

      :math:`Verify: 65536 - (188 + (254 * 256)) = 324`


----

.. index:: Pseudo Commands

Pseudo Command Syntax
=====================
Throughout this document, sample functions will be used to express actions such as writing data to the printer, calling
print and viewing the results. These are not meant to represent low-level implementations but are simply abstractions for
the purpose of providing examples.

=========== ===========
Command     Description
=========== ===========
write(data) Writes the specified data to the printer. The data may be hex, ascii, mixed, etc.
print()     Request the printer to print its buffer
`>>>`       Display result or the printer's response
=========== ===========

These examples will reside in code blocks with the important lines highlighted yellow.

:Like This:
    .. code-block:: none
        :emphasize-lines: 1

        write('pseudo write command send data to printer')
        print()
        >>> Some sort of response
