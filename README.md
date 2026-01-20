# Pyramid Thermal Printer Command Set

![](https://readthedocs.org/projects/escpos/badge/?version=latest)

Official Pyramid Technologies Thermal Printer ESC/POS Command Set Documentation

## Setup

It is recommended that you create a virtual environment for this project. Conda or venv are fine. Once configured, activate your environment before
proceeding.

For quick development, we recommend VS Code + https://github.com/vscode-restructuredtext/vscode-restructuredtext to edit and preview the docs
locally. Be sure to launch `code .` from your activate Python environment
for the VS Code plugin to work properly. Alternatively `.\make.bat html`, you can manually build and view in your browser.
Run `start .\_build\html\index.html`

## Building

    pip install docutils
    pip install sphinx_rtd_theme
    pip install sphinx==1.2.2
    pip install sphinx-autobuild

(Can't use requirements.txt as that will break the build server for some reason)

[Read on Readthedocs](http://escpos.readthedocs.io/)


## Template (RST)

 - All Hex command must be in upper ase
 - All ESC (ASCII) commands must be exactly the right case
 - Pay attention to tabs, spacing and newlines

 - Use \clearpage for forced page breaks on PDF render

 .. raw:: latex

    \clearpage

.. _tagname:
.. index:: $BYTE1 $BYTE2
.. py:attribute:: Command Name - $BYTE1 $BYTE2

    Short description

    :Format:
             ``Hex      $BYTE11B $CODE  ARGS``

             ``ASCII    ASCII CODE    ARGS``

             ``Decimal  DEC  CODE   ARGS``
    :Notes:
        - Caveats and exception
        - Go in this section of the doc
        - If you need subnotes, then indent 3x
          - Like this!

    :Range: ``None``
    :Default: ``None``
    :Related: :ref:`Description of what you're linking to <tagname>`
    :Example:
        .. code-block:: none
            :emphasize-lines: 1

            write('\x1b\x44\x00')             # Cancel previous tab settings, restores defaults
            write('\x1b\x44\x08\x14\x25\x00') # Set tab stops at 8, 20, and 32 characters
            write('Item\x09Quantity\x09Price')
            print()
            >>> Item		Quantity	Price

