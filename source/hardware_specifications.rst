.. _hardware_specification:

Hardware Specifications
=======================

Evolutions of Monocle hardware:
  * :ref:`MK11 - Second Formfactor Board <mk11_hardware>`   - **(Currently in production)**

.. _mk11_hardware:

MK11 Hardware Specification
---------------------------

Overview of monocle hardware MK11 REV board. 

:download:`MK11 Board Schematics <https://github.com/Itsbrilliantlabs/Boards/blob/main/Monocle%20main%20board%20v1.0.pdf>`

.. image:: images/block_diagram.png
  :alt: Hardware Block Diagram of Monocle.


* MCU runs on `NRF52832 <https://www.nordicsemi.com/products/nrf52832>`_ SOC.
* Monocle also has `Gowin GW1NR-9 FPGA <https://www.gowinsemi.com/en/product/detail/46/>`_ on its Data Path.

Hardware Features
-----------------

#. The data path of Camera and Mic is connected to FPGA and the output stored in SRAM. This is done infinitely from Monocle Bootup till monocle sleeps.
#. If any Touch event is detected, MCU commands FPGA to freeze the Frame buffer and Audio buffer stored in SRAM and queries the Frame to Transfer through BLE and/or Display the image to OLED.