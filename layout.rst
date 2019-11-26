.. index:: Layout
.. include:: global.rst

Layout Commands
===============

This section describes all commands that affect the layout of text in terms of spacing and margins. These
are advanced features that are not commonly by most users.

----------

.. raw:: latex

    \clearpage

.. _1b20:
.. index:: $1B $20 - Right Side Character Spacing

.. py:attribute:: Right Side Character Spacing - $1B $20

   Sets the right-side character spacing to [n × (horizontal or vertical motion unit)]. 

   :Format:
       ``Hex       $1B $20 n``  

       ``ASCII     ESC SP n``  
        
       ``Decimal   27  32 n``  
   :Notes:
       - Settings that exceed the printable area are ignored.
       - The maximum right side character spacing is 255/204 inches.  
       - The horizontal (perpendicular to paper feed) motion unit is used. 
       - The horizontal and vertical motion units are specified by :ref:`Motion Units<1d50>`. Changing the horizontal or vertical motion unit does not affect the current right-side character spacing. 
       - Right-Side character spacing is effective until it is changed, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power is turned off. 
       - When underline mode is turned on, the right side character spacing is underlined. 
       - In standard mode, right side character spacing has no effect when characters are rotated 90​°​ or 270​°.      

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``0``  
   :Related: 
       :ref:`Motion Units <1d50>`   

       :ref:`Relative Print Position <1b5c>` 

   :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1b33:
.. index:: $1B $33 - Line Spacing 

.. py:attribute:: Line Spacing - $1B $33

   Sets the line spacing to [n  (vertical or horizontal motion unit)] in inches

   :Format:
        ``Hex       $1B $33 n``  

        ``ASCII     ESC 3 n``  
        
        ``Decimal   27  51 n``  

   :Notes:
       - This command sets the line spacing in standard mode. 
       - The vertical and horizontal motion units are specified by :ref:`Motion Units <1d50>`.  
       - Changing the vertical or horizontal motion units does not affect the current line spacing. 
       - If the calculation results in a fraction, the decimal portion will be ignored. 
       - The vertical motion unit is used. 
       - Minimum line spacing  = 0.00492 inches (0.125mm) 
       - Maximum line spacing = 4 inches (101.6mm) 
       - Line spacing is effective until it is changed by another command, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power is turned off. 

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``34 (1/6"), n is base 10``
   :Related: 
    :ref:`Motion Units<1d50>` 

    :ref:`Line Spacing<1b33>`        

    :ref:`Select 1/6" Line Spacing<1b32>` 

    :ref:`Select 1/8" Line Spacing<1b30>` 

   :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1b32:
.. index:: $1B $32 - 1/6"  Line Spacing 

.. py:attribute:: Select 1/6 Inch Line Spacing - $1B $32

   Sets the line spacing to 1/6 an inch

   :Format:
       ``Hex       $1B $32``  

       ``ASCII     ESC 2``  

       ``Decimal   27  50``  

   :Notes:
       - This command sets the line spacing in standard mode.  
       - Line spacing is effective until it is changed by another command, :ref:`Initialize <1b40>` is executed, the printer is reset, or the power is turned off. 

   :Range: ``None``
   :Default: ``None``
   :Related: 
       :ref:`Select 1/8" Line Spacing<1b30>`  
       
       :ref:`Motion Units<1d50>` 
       
       :ref:`Line Spacing<1b33>`        

    :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1b30:
.. index:: $1B $30 - 1/8"  Line Spacing 

.. py:attribute:: Select 1/8 Inch Line Spacing - $1B $30

    Sets the line spacing to 1/8 an inch

   :Format:
        ``Hex       $1B $30``  

        ``ASCII     ESC 1``  
        
        ``Decimal   27  48`` 

   :Notes:
       - This command sets the line spacing in standard mode.  
       - Line spacing is effective until it is changed by another command, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power is turned off. 

   :Range: ``None``
   :Default: ``None``
   :Related:
       :ref:`1/6 inch spacing<1b32>` 
       
       :ref:`Motion Units<1d50>` 
       
       :ref:`Line Spacing<1b33>`   

   :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1b61:
.. index:: $1B $61 - Select Justification 

.. py:attribute:: Select Justification - $1B $61

   Select justification mode

   :Format:
        ``Hex       $1B $61 n``  

        ``ASCII     ESC a   n``  
        
        ``Decimal   27  97  n`` 

   :Notes:
       - This command is enabled only when processed at the beginning of a line. (When the current line is empty) 
       - This command applies the justification within the area set by :ref:`Left Margin<1d4c>` ​and :ref:`Print Area Width<1d57>`
       - This command will justify all data in the printing area such as characters, graphics, bit images, barcode and space area set by :ref:`Horizontal Tab<x09>`, :ref:`Absolute<1b24>` and :ref:`Relative<1b5c>` print positions. 
       - Settings of this command are effective until ​the :ref:`Initialize <1b40>` command is executed, the printer is reset, or the power is turned off.        
        - When n=0 or 48, left justification is enabled 
        - When n=1 or 49, center justification is enabled 
        - When n=2 or 50, right justification is enabled         
   
   :Range: ``n=0,1,2,48,49,50``
   :Default: ``0``
   :Related:  
            :ref:`Left Margin <1d4c>`                

            :ref:`Print Area Width <1d57>`   

            :ref:`Absolute Print Position <1b24>`  

            :ref:`Relative Print Position <1b5c>`                              

            :ref:`Horizontal Tab <x09>`   

   :Example:
      |justification|

---

.. raw:: latex

    \clearpage

.. _1d4c:
.. index:: $1D $4C - Left Margin 

.. py:attribute:: Left Margin - $1D $4C

    Set left margin

   :Format:
        ``Hex       $1D $4C nL nH``  

        ``ASCII     GL  L   nL nH``  
        
        ``Decimal   27  76  nL nH``     

   :Notes:
       - In standard mode, sets the left margin to [(nL + (nH × 256)) × (horizontal motion unit)] from the left edge of the printable area. Uses :ref:`Two Byte Number Definitions<2byte>`. 
       - This command is enabled only when processed at the beginning of a line. (When the current line is empty) 
       - This command applies the justification within the area set by :ref:`Left Margin<1d4c>` ​and :ref:`Print Area Width<1d57>`
       - This command will justify all data in the printing area such as characters, graphics, bit images, bar code and space area set by :ref:`Horizontal Tab<x09>`, :ref:`Absolute<1b24>` and :ref:`Relative<1b5c>` print positions. 
       - Settings of this command are effective until :ref:`Initialize <1b40>` is executed, the printer is reset, or the power is turned off. 
    
   |leftmargin|

   :Range: ``0 ≤ nL, nH ≥ 255, 0 ≤ (nL + (nH × 256))≤ 65535``
   :Default: ``nL = 0, nH =0``
   :Related: 
       :ref:`Motion Units<1d50>` 

       :ref:`Print Area Width <1d57>`                              

   :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1d50:
.. index:: $1D $50 - Motion Units 

.. py:attribute:: Motion Units - $1D $50

    Set horizontal and vertical motion units

   :Format:
        ``Hex       $1D $50 x   y``  

        ``ASCII     GS  P   x   y``  
        
        ``Decimal   29  80  x   y``     

   :Notes:        
       - Sets the horizontal and vertical motion units to approximately 25.4/x mm {1/x"} and approximately 25.4/y mm {1/y"}, respectively.

        - When x = 0, the default value of the horizontal motion unit is used. 
        - When y = 0, the default value of the vertical motion unit is used.
        - When x > 204, the default value of the horizontal motion unit is used.
        - When y > 204, the default value of the vertical motion unit is used.

       - The horizontal direction is perpendicular to the paper feed direction and the vertical direction is the paper feed direction. 
       - The horizontal and vertical motion units indicate the minimum pitch used for calculating the values of related commands 
       - In standard mode, the following commands use x or y. 

         - Commands using x: :ref:`Left Margin <1d4c>`, :ref:`Print Area Width <1d57>` 
         - Commands using y

       - If the result is a decimal number, the decimal is ignored. 
       - This command does not affect the previously defined values for settings that use the horizontal or vertical motion units. 
       - Settings of this command are effective until it is changed, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power is turned off. 


   :Range: ``0 ≤ x, y ≤ 204``
   :Default: ``x = 204, y = 204``
   :Related: 
       :ref:`Left Margin <1d4c>`

       :ref:`Print Area Width <1d57>`       

   :Example: ``None``

----

.. raw:: latex

    \clearpage

.. _1d57:
.. index:: $1D $57 - Print Area Width 

.. py:attribute:: Print Area Width  - $1D $57

    Set print area width 

   :Format:
        ``Hex       $1D $57 nL  nH``  

        ``ASCII     GS  W   nL  nH``  
        
        ``Decimal   29  87  nL  nH``     

   :Notes:        
       - In standard mode, sets the print area width  to [(nL + (nH × 256)) × (horizontal motion unit)] from the right edge of the left margin. See :ref:`Two-byte Numbers<2byte>`
       - This command is only executed when the printer is in a :ref:`New Line State<newlinestate>`
       - If the setting exceeds the printable area, the print area width is automatically set to the maximum value of the printable area. 
       - If the [left margin + print area width] is greater than the printable area, the print area will be truncated automatically to [printable area - left margin]. If the left margin is changed, the print area width will also change until there is room to fit the specified print area width. 
       - If the print area width equals 0, then the print area width is automatically set to the maximum value of the printable area. 
       - The horizontal (perpendicular to paper feed direction) motion unit is used to set print area width. 
       - The horizontal and vertical motion units are specified by :ref:`Motion Units<1d50>`. Changing the horizontal or vertical motion unit does not affect the current print area width. 
       - Settings of this command are effective until it is changed, :ref:`Initialize<1b40>` is executed, the printer is reset, or the power is turned off. 

   :Range: ``0 ≤ nL, nH ≥ 255, 0 ≤ (nL + (nH × 256))≤ 65535``
   :Default: ``nL = 64, nH = 2``
   :Related: 
       :ref:`Motion Units<1d50>`

       :ref:`Print Area Width<1d57>`       

   :Example: |printablearea|
