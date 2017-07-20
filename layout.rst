.. index:: Layout
.. include:: global.rst

Layout Commands
===============

This section describes all commands that affect the layout of text in terms of spacing and margins. These
are advanced features that are not commonly by most users.

----------
.. _1b20:
Right Side Character Spacing - $1b $20
--------------------------------------

.. py:attribute:: Right Side Character Spacing - $1b $20

   Sets the right-side character spacing to [n × (horizontal or vertical motion unit)]. 

   :Format:
        ``Hex       $1B $20 n``  

        ``ASCII     ESC SP n``  
        
        ``Decimal   27  32 n``  
   :Notes:
       - Settings that exceed the printable area are ignored.
       - The maximum right side character spacing is 255/204 inches.  
       - The horizontal (perpendicular to paper feed) motion unit is used. 
       - The horizontal and vertical motion units are specified by $1D $50. Changing the horizontal or vertical motion unit does not affect the current right-side character spacing. 
       - Right-Side character spacing is effective until it is changed, ​ESC @​ is executed, the printer is reset, or the power is turned off. 
       - When underline mode is turned on, the right side character spacing is underlined. 
       - In standard mode, right side character spacing has no effect when characters are rotated 90​°​ or 270​°.      

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``0``  
   :Related: 
       :ref:`Motion Units <1d50>`   

       :ref:`Reliative Print Position <1b5c>` 

   :Example: ``None``

----
.. _1b33:
Line Spacing Spacing - $1b $33
------------------------------

.. py:attribute:: Line Spacing Spacing - $1b $33

   Sets the line spacing to [n  (vertical or horizontal motion unit)] in inches ×

   :Format:
        ``Hex       $1B $33 n``  

        ``ASCII     ESC 3 n``  
        
        ``Decimal   27  51 n``  

   :Notes:
       - This command sets the line spacing in standard mode. 
       - The vertical and horizontal motion units are specified by ​$1D $50​. Changing the vertical or horizontal motion units does not affect the current line spacing. 
       - If the calculation results in a fraction, the decimal portion will be ignored. 
       - The vertical motion unit is used. 
       - Minimum line spacing  = 0.00492 inches (0.125mm) 
       - Maximum line spacing = 4 inches (101.6mm) 
       - Line spacing is effective until it is changed by another command, ​ESC @​ is executed, the printer is reset, or the power is turned off. 

   :Range: ``0 ≤ n ≤ 255``
   :Default: ``34 (1/7"), n is base 10``
   :Related: 
    :ref:`Motion Units<1d50>` 

    :ref:`Line Spacing<1b33>`        

    :ref:`Select 1/6" Line Spacing<1b32>` 

    :ref:`Select 1/8" Line Spacing<1b30>` 

   :Example: ``None``

----
.. _1b32:
Select 1/6 Inch Line Spacing - $1b $32
----------------------------

.. py:attribute:: Select 1/6 Inch Line Spacing - $1b $32

    Sets the line spacing to 1/6 an inch

   :Format:
        ``Hex       $1B $32``  

        ``ASCII     ESC 2``  
        
        ``Decimal   27  50``  

    :Notes:
        - This command sets the line spacing in standard mode.  
        - Line spacing is effective until it is changed by another command, ​ESC @​ is executed, the printer is reset, or the power is turned off. 

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Select 1/8" Line Spacing<1b30>`  

              :ref:`Motion Units<1d50>` 

              :ref:`Line Spacing<1b33>`        

    :Example: ``None``

----
.. _1b30:
Select 1/8 Inch Line Spacing Spacing - $1b $30
----------------------------------------------

.. py:attribute:: Select 1/8 Inch Line Spacing - $1b $30

    Sets the line spacing to 1/8 an inch

   :Format:
        ``Hex       $1B $30``  

        ``ASCII     ESC 1``  
        
        ``Decimal   27  48`` 

   :Notes:
       - This command sets the line spacing in standard mode.  
       - Line spacing is effective until it is changed by another command, ​ESC @​ is executed, the printer is reset, or the power is turned off. 

   :Range: ``None``
   :Default: ``None``
   :Related:
       :ref:`1/6 inch spacing<1b32>` 
       
       :ref:`Motion Units<1d50>` 
       
       :ref:`Line Spacing<1b33>`   

   :Example: ``None``

----
.. _1b61:
Select Justification - $1b $61
------------------------------

.. py:attribute:: Select Justification - $1b $61

   Select justification mode

   :Format:
        ``Hex       $1B $61 n``  

        ``ASCII     ESC a   n``  
        
        ``Decimal   27  97  n`` 

   :Notes:
       - This command is enabled only when processed at the beginning of a line. (When the current line is empty) 
       - This command applies the justification within the area set by ​$1D $4C ​and $1D $57 
       - This command will justify all data in the printing area such as characters, graphics, bit images, bar code and space area set by ​$09, $1B $24 and $1B $5C​. 
       - Settings of this command are effective until ​:ref:`initialize <1b40>`​ is executed, the printer is reset, or the power is turned off. 
   
   :Range: ``n=0,1,2,48,49,50``
   :Default: ``0``
   :Related:  
            :ref:`TODO <1d4c>`                

            :ref:`Print Area Width <1d57>`   

            :ref:`TODO <1b24>`  

            :ref:`TODO <1b5c>`                              

            :ref:`TODO <x9>`   

   :Example:
      |justification|

----
.. _1b61:
Left Margin - $1d $4c
------------------------------

.. py:attribute:: Left Margin - $1d $4c

    Set left margin

   :Format:
        ``Hex       $1D $4C nL nH``  

        ``ASCII     GL  L   nL nH``  
        
        ``Decimal   27  76  nL nH``     

   :Notes:
       - In standard mode, sets the left margin to [(nL + (nH × 256)) × (horizontal motion unit)] from the left edge of the printable area. Uses Two Byte Number Definitions. See ​:ref:`Two Byte Math<2byte>`
       - This command is enabled only when processed at the beginning of a line. (When the current line is empty) 
       - This command applies the justification within the area set by ​$1D $4C ​and $1D $57 
       - This command will justify all data in the printing area such as characters, graphics, bit images, bar code and space area set by ​$09, $1B $24 and $1B $5C​. 
       - Settings of this command are effective until ​$1b $40​ is executed, the printer is reset, or the power is turned off. 
    
   |leftmargin|

   :Range: ``0 ≤ nL, nH ≥ 255, 0 ≤ (nL + (nH × 256))≤ 65535``
   :Default: ``nL = 0, nH =0``
   :Related: 
       :ref:`Motion Units<1d50>` 

       :ref:`Print Area Width <1d57>`                              

   :Example: ``None``
   
----

.. _1d50:
Motion Units - $1d $50
------------------------------

.. py:attribute:: Motion Units - $1d $50

    Set horizontal and vertical motion units

----

.. _1d57:
Print Area Width  - $1d $57
------------------------------

.. py:attribute:: Print Area Width  - $1d $57

    Set print area width 

----