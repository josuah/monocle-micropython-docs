.. |date| date::
.. |time| date:: %H:%M

Introduction to Monocle
=======================
.. figure:: /images/monocle.png

   *Monocle.*

Augmented Reality (AR) is no longer limited to laboratories and fieldwork.
Monocle is a pocket-sized device that clips right to your glasses.
What it does best is help you collect memories.
Share your clips and photos.
Zoom in on far away objects.
All without ever having to take your phone out of your pocket.
Live life without looking down at your phone.

Visit `Monocle <https://www.itsbrilliant.co/>`_ to get hands on one.
And `Discord <https://discord.gg/3YvPv8tDMj>`_ channel for any new updates.

Audience
--------
This document is mainly for software developers to the opensource community and testers who are contributing to build firmware,
FPGA and Mobile application for the monocle product.

Future Changes to Monocle
-------------------------

Monocle Firmware Side
^^^^^^^^^^^^^^^^^^^^^
* Audio transfer from Monocle Hardware to Phone Application
* Reliable transfer of data to phone
* Data tranfer from Phone to Monocle Hardware
* FPGA Upgrade feature

Monocle FPGA Side
^^^^^^^^^^^^^^^^^
* Video Compression for faster Video transfer from Monocle to Phone

Monocle Phone Application Side
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Application on phone displaying Video/Photo transferred from monocle


.. toctree::
   :maxdepth: 5
   :caption: Hardware

   product_specifications
   how_to_devboard
   future

.. toctree::
   :maxdepth: 5
   :caption: Firmware

   how_to_firmware
   software_architecture
   software_design
   html/index.rst

.. toctree::
   :maxdepth: 5
   :caption: FPGA

   how_to_fpga
   hardware_specifications

.. toctree::
   :maxdepth: 5
   :caption: Phone App

   how_to_mobile
   phone_app

.. toctree::
   :maxdepth: 5
   :caption: Misc

   documentation_tools

.. toctree::
   :maxdepth: 5
   :caption: Release

   release_notes_firmware
   release_notes_phone

Generated on |date| at |time|.
