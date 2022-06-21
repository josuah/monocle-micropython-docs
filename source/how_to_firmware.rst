.. _how_to_firmware:

Get Started with Firmware on MCU
================================

This section describes all the necessary steps a developer wants to get start with monocle firmware development.

Setup the Environment on your Windows Machine
---------------------------------------------

#. Install `Segger Embedded Studio <https://www.segger.com/downloads/embedded-studio>`_ V5.44

   * **(Optional)** `Licensing <https://www.segger.com/products/development-tools/embedded-studio/license/licensing-conditions/>`_ 
      * Request a `Nordic license <https://license.segger.com/Nordic.cgi>`_
      * Receive emailed license
      * 'Tools' -> 'License Manager...' and click 'Activate Embedded Studio' 

   * Add software packs for Cortex-ARM and nRF
      * Tools -> Package Manager
      * Yes to get latest list
      * Find CMSIS-CORE Support Package (not CMSIS 5 CMSIS-CORE Support Package)
      * Right-click to get context menu, and select Install Selected Package
      * the Action column will change from No Action to Install
      * Scroll down to find Nordic nRF CPU Support Package
      * Right-click and select as above (note: this will also select the CMSIS 5 CMSIS-CORE Support Package)
      * Click Next button to download & install the 3 packages
      * Click Finish

   * **(Optional)** Enable the CMSIS Configuration Wizard (provides graphical ``sdk_config.h`` file editing)

      * `Download <https://java.com/en/download/>`_ and install Java
      * See instructions `click here <https://infocenter.nordicsemi.com/index.jsp?topic=%2Fsdk_nrf5_v17.0.2%2Fsdk_config.html&cp=7_1_1_6_5_1&anchor=sdk_config_ide_ses>`_ & helpful video `here <https://www.youtube.com/watch?v=b0MxWaAjMco>`_
      * (will need to restart SES for tool to become available)
      * (if using in own project, may need to see `this <https://devzone.nordicsemi.com/f/nordic-q-a/27765/sdk_config-wizard-in-segger-ses>`_) instructions

#. Install `nRF5 SDK <https://www.nordicsemi.com/Software-and-tools/Software/nRF5-SDK/Download>`_ v17.0.2 + Softdevice S132

   * Unizp the SDK archive (this will take ~1 hour)
   * **(Important)** Place it in the location of your choice, suggested: ``C:\Nordic\``


Clone Monocle git
-----------------

.. code::

   git clone git@github.com:Itsbrilliantlabs/monocle-firmware.git

Folder Structure of git
-----------------------

.. code::

   ./monocle_firmware
   ├── lib
   ├── mk11
   ├── swl_release
   └── (other source files ...)

* ``lib/`` contains all libraries
* ``mk11/`` contains MK11 board's segger embedded-studio project files and  configurations
* ``swl_release/`` contains Swaralink's BLE stack. Note: This has a separate Licensing terms
* ``(other source files ...)`` all other source files

Build the code
--------------
Navigate to, and double-click on the SES project file for the hardware you wish to build for

* For example, for MK11, double click: ``C:\Nordic\nRF5_SDK_17.0.2_d674dde\examples\monocle\mcu\mk11\s132\ses\monocle+ble_mk11_s132.emProject``
* In Build menu, select the first action (yellow bricks icon, F7 hotkey) to build the code
* If all is well, it should build without errors or warnings

How to run the code on the Monocle Hardware
-------------------------------------------
Update the Monocle firmware using Brilliant Mobile Application. Make sure the Bluetooth is paired with Monocle Hardware.

Follow the steps shown below:

1. Go to Settings and click on "Upgrade Firmware" Button.

.. image:: images/FwUpgd1.png
  :scale: 50 %
  :alt: Firwmare Upgrade Screenshot 1


2. Click on  "Select Update File". Your File manager will open, select the "zip" file.

.. image:: images/FwUpgd2.png
  :scale: 50 %
  :alt: Firwmare Upgrade Screenshot 2


3. Click on "Perform Firmware Update"

.. image:: images/FwUpgd3.png
  :scale: 50 %
  :alt: Firwmare Upgrade Screenshot 3


4. Wait for the update to complete.

.. image:: images/FwUpgd4.png
  :scale: 65 %
  :alt: Firwmare Upgrade Screenshot 4