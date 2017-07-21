.. index:: Movement

Paper Movement Commands
==========================

This section describes all commands that move the ticket from an idle to print state. This include cuts, ejection,
presenting and retracting.

----------

.. raw:: latex

    \clearpage

.. _1b69:
.. index:: $1B $69 - Total Cut

.. py:attribute:: Total Cut - $1B $69

   Performs a full cut on the current ticket.

   :Notes:
       - If a ticket is not at least the minimum ticket size, then a blank portion will be printed/added to the ticket to make it the minimum size before the cut.
       - If nothing has been printed, then the command is ignored.

   :Format:
       ``Hex       $1B $69``  

       ``ASCII     ESC  i``  

       ``Decimal   27   105``

   :Range: ``None``
   :Default: ``None``
   :Related: :ref:`Form Feed<x0c>`
