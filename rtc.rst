.. index:: RTC Commands
.. include:: global.rst

RTC Commands
================

This section describes all commands that affect the layout of text in terms of spacing and margins. These
are advanced features that are not commonly by most users.

----------

.. raw:: latex

    \clearpage

.. _1c7d70:
.. Index:: $1C $7D $70 - Print Real Time Clock

Print Real Time Clock - ``$1C $7D $70`` |phx|
-------------------------------------

   Prints the real-time clock.

   :Format:
       ``Hex       $1C  $7D $70 n``

       ``ASCII     GS   }   p``

       ``Decimal   28  125  112 n``

   :Notes:
       - This command will feed the real time clock as a string to the ESC/POS print buffer. The arguments will determine the format of the output.

   :Range: ``n = 0, 1, 2, 3, 4``

   == =====================================
   n  RTC Control Byte
   == =====================================
   0  Print time/date in standard time
   1  Print just time
   2  Print just date
   3  Print time/date in dd/mm/yyyy format
   4  Print just date in dd/mm/yyyy format
   == =====================================
