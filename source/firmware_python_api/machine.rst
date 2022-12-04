:py:mod:`machine`
-----------------

.. py:module:: machine

.. py:class:: Battery

   .. py:function:: level

      Returns the current battery level as a percentage
   
.. py:class:: Camera

   .. py:function:: capture(url)

      Sends the last captured image over WiFi to the given URL. If no URL is provided, the image is sent over bluetooth
   
   .. py:function:: stream(url)

      Starts streaming images over WiFi to the given URL. If no URL is provided, the images are sent over bluetooth
   
   .. py:function:: stop()

      Stops any ongoing image stream or capture currently in progress

.. py:class:: Display

   .. py:function:: show(framebuffer)

      Pushes a framebuffer object to the FPGA for drawing onto the display


.. py:class:: FPGA

   .. py:function:: download(url)

      Downloads and reboots the FPGA with the bitstream from the url provided. If a url is not provided, the bitstream is requested over bluetooth. Automatically powers up the FPGA if it was powered down

   .. py:function:: spi_read(cmd, addr, bytes)

      Reads n bytes from the FPGA with a given command and address. Returns a byte array

   .. py:function:: spi_write(cmd, addr, bytes, buffer)

      Writes n bytes from buffer to the FPGA with a given command and address. 

   .. py:function:: status()

      Returns the current status of the FPGA


.. py:class:: Microphone

   .. py:function:: stream(*url*)

      Starts streaming audio data over WiFi to the given URL. If no URL is provided, the audio is sent over bluetooth

   .. py:function:: stop()

      Stops any ongoing audio stream


.. py:class:: Power

   .. py:function:: hibernate(enable)

      Enables or disables all the high power devices. Networking remains active. Upon re-enabling the FPGA will remain in reset until booted using FPGA.boot()

   .. py:function:: reset()

      Resets the device

   .. py:function:: reset_cause()

      Returns the reason for the previous reset or startup state

   .. py:function:: shutdown(timeout)

      Places the device into deep-sleep and powers down all high power devices. If a timeout is given, the device will wake up again after that many seconds, otherwise the device will only wake up upon inserting, and removing from the case. Upon wakeup, the device will reset, and the cause can be seen using the Power.reset_cause() function


.. py:class:: Timer(id, period, callback, oneshot)

   Creates a new Timer object on timer id with the period in milliseconds and a given callback handler.
   The oneshot value can optionally be set to true if only a single trigger is required.
   By default the timer is repeating

   .. py:function:: value()

      Returns the current count value of the timer in milliseconds

   .. py:function:: deinit()

      De-initializes the timer and stops any callbacks


.. py:class:: Touch

   .. py:function:: mac_address()

      :returns: the 48bit MAC address of the device as a 17 character string. Each byte is delimited with a colon

   .. py:function:: update(start)

      Checks for firmware updates and returns True if it is available.
      If start is set to True, the update process is begun, and the device will enter the bootloader state
