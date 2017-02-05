.. index:: Movement

Paper Movement Commands
==========================

This section describes all commands that move the ticket from an idle to print state. This include cuts, ejection,
presenting and retracting.

----------

.. _1b69:
.. py:attribute:: Total Cut - $1B $69

    Performs a full cut on the current ticket.

    :Notes:
        - If a ticket is not at least the minimum ticket size, then a blank portion will be printed/added to the ticket to make it the minimum size before the cut.
        - If nothing has been printed, then the command is ignored.


    :Format: ``$1B $69`` or ``ESC i`` or ``27 105``
    :Range: ``None``
    :Default: ``None``
    :Related: ``None``
