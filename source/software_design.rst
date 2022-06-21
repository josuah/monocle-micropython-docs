.. _software_design:

.. toctree::
   :hidden:

   hardware_specifications
   software_architecture

Software Design
===============

This section describes design details of Monocle Firmware on the High level architecture discussed in the previous section. This section goes deep in the architecture to understand how each module/component is implemented.

Data structure
--------------
1. Events to be Handled are mentioned below:

::

   MONOCLE_EVENT_G_TAP
   MONOCLE_EVENT_G_DOUBLE_TAP
   MONOCLE_EVENT_G_PRESS
   MONOCLE_EVENT_G_LONGPRESS
   MONOCLE_EVENT_G_LONGBOTH
   MONOCLE_EVENT_TIMEOUT
   MONOCLE_EVENT_BATT_LOW
   MONOCLE_EVENT_BATT_CHARGE
   MONOCLE_EVENT_BLE_CONNECT
   MONOCLE_EVENT_BLE_DISCONNECT
   MONOCLE_EVENT_BLE_TRANSFER_DONE

2. States of Monocle Firmware.

::

    MONOCLE_EARLY_INIT
    MONOCLE_FULL_INIT
    MONOCLE_POWERED_DOWN
    MONOCLE_SHUTDOWN_PENDING
    MONOCLE_RECORD_D1
    MONOCLE_RECORD_D0
    MONOCLE_PHOTO_D1
    MONOCLE_PHOTO_D0
    MONOCLE_REPLAY_D1
    MONOCLE_REPLAY_D0

3. monocle_state variable in main.c represents the current state of monocle firmware.

I2C Module Design
-----------------
One of the interface to achive High Speed Inter Module comminication is via TWO Wire interface such as I2C. In Monocle :ref:`MK11 <hardware_specification>` form factor board, there are 2 I2C interfaces. 
I2C Interfaces are connected to 3 main modules.

* Camera Module (OV5640) Connected on I2C1, Slave Address 0x3C.
* Touch Module (IQS620A) Connected on I2C0, Slace Address 0x44.
* Power Module (MAX77654) Connected on I2C0, Slave Address 0x48.

1. Initializing each I2C instaces are done using the below ``init`` APIs. This API typically initializes the 2 I2C interfaces appropriately to communicate to the corresponding attached mocules.

APIs for I2C0 and I2C1 respectively:
:: 

  void i2c_init    (void);
  void i2c_sw_init (void);

* *Params:*
   * None
* *Return*
   * None

2. Writing to a module which are connected to I2C Interface are done using the below ``write`` APIs.

APIs for I2C0 and I2C1 respectively:
::

  bool i2c_write    (uint8_t slaveAddress, uint8_t *writeBuffer, uint8_t numberOfBytes);
  bool i2c_sw_write (uint8_t slaveAddress, uint8_t *writeBuffer, uint8_t numberOfBytes);

* *Params*
   * slaveAddress  -> Unique Address of the Module to be written to.
   * writeBuffer   -> Write Data Buffer.
   * numberOfBytes -> Number of bytes to be written.

3. Reading from a mocule which are connected to I2C interface are done using the below ``read`` APIs.

APIs for I2C0 and I2C1 respectively:
::

  bool i2c_read    (uint8_t slaveAddress, uint8_t *readBuffer, uint8_t numberOfBytes);
  bool i2c_sw_read (uint8_t slaveAddress, uint8_t *readBuffer, uint8_t numberOfBytes);

* *Params*
   * slaveAddress  -> Unique Address of the Module to be read from.
   * readBuffer    -> Read Data Buffer.
   * numberOfBytes -> Number of bytes to be read.

SPI Module Design
-----------------
The Serial Peripheral interface (SPI) is connected to 3 Modules internally in Monocle MK11 Board. The MCU acts as Master always and the other modules as Slave.

* FPGA connected with Chip Select GPIO PIN 8
* OLED connected with Chip Select GPIO PIN 6
* FLASH connected with Chip Select GPIO PIN 4 (Not accessed in the Firmware)

1. Initializing SPI instace is using the below ``init`` APIs. After executing this API the SPI Interface should be configured as Master and be ready to communicate to each module. 
::

  void spi_init (void);

* *Params:*
   * None
* *Return*
   * None

2. Selecting the CHIP Select PIN. This API is called before calling ``read`` or ``write`` to any module.
::

  void spi_set_cs_pin (uint8_t cs_pin);

* *Params*
   * cs_pin -> CS PIN Number of the corresponding module connected to SPI Interface.

3. Writing to a module which are connected to SPI Interface is done using ``write`` API.
::

  void spi_write_burst (uint8_t addr, const uint8_t *data, uint16_t length);

* *Params*
   * addr   -> Write the Data starting from this Address.
   * data   -> Data Buffer.
   * length -> Data Length to be written.

4. Reading to a module which are connected to SPI Interface is done using ``read`` API
::

  uint8_t *spi_read_burst (uint8_t addr, uint16_t length);

* *Params:*
   * addr   -> Read the data from this Address.
   * length -> Length of data to be read.
* *Return*
   * Read Data Buffer.

ADC Module Design
-----------------
ADC on Monocle Hardware is used for sampling the Battery Voltage reading. Every 1 Sec the ADC is kick-started to sample the Battery Voltage. The Voltage is averaged for 5 sec or 5 Samples.

There are 2 types of APIs is ADC. 

* Quick APIs  -> Used only to calculate Battery Voltage quickly in the initial bootup of Monocle Firmware.
* Full APIs -> Used to calculate Battery Voltage in the Normal operation.

1. Init ADC API: To initialize the ADC Sampling intervals with NRF SDK.
::

  void adc_init       (void);
  void adc_quick_init (void);

* *Params:*
   * None
* *Return*
   * None

2. Deinitializing ADC APIs: Deinitializing is only required in the initial phase of the Firmware bootup.
::

  void adc_quick_uninit(void)

* *Params:*
   * None
* *Return*
   * None

3. ADC Start Sampling: This API is called every Second to check the Battery Voltage
::

  void adc_start_sample (void);

* *Params:*
   * None
* *Return*
   * None

4. API to get the present Battery Voltage.
::

  float adc_get_batt_voltage      (void);
  float adc_quick_get_batt_voltage(void)

* *Params:*
   * None
* *Return*
   * ADC Battery Voltage

4. API To get the present Battery Percentage.
::
  
  uint8_t adc_get_batt_soc(void)

* *Params:*
   * None
* *Return*
   * ADC Battery Percentage

FPGA Interface Design
---------------------
The FPGA is interfaced with SPI with the MCU. FPGA Interface modues is the highlevel APIs to get the FPGA LEDs, Mic, Camera and OLED functionality to MCU.  

Refer the latest  `FPGA Register specification. <https://docs.google.com/spreadsheets/d/1vyFDFN8B-sDCPFmd3DwrnBesxnphELEp/edit#gid=1239914432>`_

Following are the APIs defined for the same.

1. **FPGA read / write APIs.**
These APIs are interfaced through SPI read/write funtions. Any FPGA register can be read or written using these functions.
::

  void fpga_write_byte (uint8_t addr, uint8_t data);

* *Params:*
   * addr -> FPGA Register Address.
   * data -> 1 Byte Data to be written to FPGA Register.
* *Return*
   * None

::

  void fpga_write_burst (const uint8_t *data, uint16_t length);

* *Params:*
   * data   -> Data to be written to FPGA Register.
   * Length ->
* *Return*
   * None

::

  uint8_t fpga_read_byte (uint8_t addr);

* *Params:*
   * addr -> 
* *Return*
   * 1 Byte Data read.

::

  uint8_t *fpga_read_burst (uint16_t length);

* *Params:*
   * Length ->
* *Return*
   * Array of data read.

2. **FPGA Control APIs**: 
::

  void fpga_soft_reset (void);
  bool fpga_xclk_on    (void);

3. **FPGA Test APIs**:
::

  bool fpga_test_reset            (void);
  bool fpga_ram_check             (void);
  bool fpga_spi_exercise_register (uint8_t addr);
  void fpga_set_luma              (bool turn_on);

3. **FPGA LED APIs**: 
On MK9B and MK10 evaluation boards there are physical LEDs that MCU can toggle. On :ref:`MK11 Form Factor Board <mk11_hardware>` there are no Physical LEDs.
::

  void fpga_led_on      (uint8_t led_number);
  void fpga_led_off     (uint8_t led_number);
  void fpga_led_toggle  (uint8_t led_number);
  void fpga_led_on_all  (void);
  void fpga_led_off_all (void);

* *Params:*
   * led_number
* *Return*
   * None

4. **FPGA Camera APIs**:  
::

  bool fpga_camera_on     (void);
  void fpga_image_capture (void);
  void fpga_video_capture (void);
  void fpga_set_zoom      (uint8_t level);


5. **FPGA Mic APIs**:
::

  bool fpga_mic_on          (void);
  bool fpga_mic_off         (void);
  void fpga_prep_read_audio (void);

6. **FPGA OLED APIs**:
::

  void fpga_disp_live   (void);
  void fpga_disp_busy   (void);
  void fpga_disp_bars   (void);
  void fpga_disp_off    (void);
  void fpga_replay_rate (uint8_t repeat);
  void fpga_set_display (uint8_t mode);

7. **FPGA Buffer APIs**:
::

  uint32_t fpga_get_capture_size (void);
  uint32_t fpga_get_bytes_read   (void);
  uint16_t fpga_get_checksum     (void);
  uint16_t fpga_get_and_clear_checksum(void);
  bool fpga_is_buffer_at_start  (void);
  bool fpga_is_buffer_read_done (void);

8. **MCU Checksum APIs**.
::

  uint16_t fpga_calc_checksum (uint8_t *bytearray, uint32_t length);
  uint16_t fpga_checksum_add  (uint16_t checksum1, uint16_t checksum2);

iQS620 Touch Module Design
--------------------------
Touch IC are different on different hardware. On :ref:`MK11 Form Factor Board <mk11_hardware>` iQS620 is the touch IC. 
There are 2 files in focus for the Touch Module design. 

* iqs620.c -> defines the low level functions to interface with IQS620 IC using I2C interface. (Not explained in this document)
* touch.c  -> defines High level functions to start or stop the TouchIC. (APIs explained below)

.. figure:: /images/mcu_touch_interface.png

*MCU to IQS620 connection on the board*

The interrupt line (IO_TOUCHED_PIN GPIO Number 2) is the indication to MCU that there is an event in IQS620 IC. Once the Interrupt occurs, MCU should quiry using I2C to get what event was it. There are 2 Buttons on MK11. Both the touch instance are indicated using this IC.

Below are the Events that will be generated.

* TOUCH_GESTURE_TAP        : 0.25 Sec
* TOUCH_GESTURE_DOUBLETAP  : 0.25 Sec (Both Tapped)
* TOUCH_GESTURE_PRESS      : 0.5 Sec
* TOUCH_GESTURE_LONGPRESS  : 9.5 Sec
* TOUCH_GESTURE_LONGBOTH   : 9.5 Sec (Both Pressed)

Refer :ref:`Gesture State machine <software_architecture>` to know more about the what actions are taken in each Gesture types.

1. **Touch Init API**: Initializes Touch IC. Quick Init API initializes the Interrupt GPIO. And the Full Init initializes the remaining intialization like statemachine and timer.
:: 

   void touch_quick_init (void);
   bool touch_init       (touch_gesture_handler_t handler);

* *Params:*
   * handler -> Callback handler to indicate the Gesture events.
* *Return*
   * None

2. **Reset function:** To reprogram the IC.
::

   bool touch_reprogram (void);

* *Params:*
   * None
* *Return*
   * None

3. **Internal ISR and Statemachine functions**
::

   static void touch_pin_handler   (void *iqs620, iqs620_button_t button, iqs620_event_t event);
   static void touch_event_handler (bool istimer);

OV5640 Camera Module Design
---------------------------
Camera module is connected and controlled by MCU using I2C and the Data path is connected to FPGA. The following image shows the same. 

.. figure:: /images/mcu_camera_interface.png

*MCU to OV5640 connection on the board*

Few important points about Camera module:

* Camera resolution: 5MP
* Records in 15 FPS. While replaying the captured buffer, FPGA replays the same frame thrice to match the 50 FPS rate on OLED.
* Camera Outputs 640X400 resolution frames by 4x Digital Zoom.
* Register configuration of camera is done by MCU.

Interface files: 

* ov5640.h    -> APIs exposed to Application (main.c).
* ov5640cfg.h -> Internal Config Header files.
* ov5640af.h  -> Internal Config Header files.

**Camera Bootup APIs**:
::

   bool ov5640_init      (void);
   bool ov5640_pwr_on    (void);
   void ov5640_pwr_sleep (void);
   void ov5640_pwr_wake  (void);

**Camera Control APIs**:
::

   void ov5640_YUV422_mode (void);
   void ov5640_mode_1x     (void);
   void ov5640_mode_2x     (void);
   void ov5640_reduce_size(uint16_t Hpixels, uint16_t Vpixels);
   void ov5640_light_mode       (uint8_t mode);
   void ov5640_color_saturation (uint8_t sat);
   void ov5640_brightness       (uint8_t bright);
   void ov5640_contrast         (uint8_t contrast);
   void ov5640_sharpness        (uint8_t sharp);
   void ov5640_special_effects  (uint8_t eft);
   void ov5640_test_pattern     (uint8_t mode);
   void ov5640_flash_ctrl       (uint8_t sw);
   void ov5640_mirror           (uint8_t on);
   void ov5640_flip             (uint8_t on);
   bool ov5640_outsize_set  (uint16_t offx, uint16_t offy, uint16_t width, uint16_t height);
   bool ov5640_imagewin_set (uint16_t offx, uint16_t offy, uint16_t width, uint16_t height); 
   bool ov5640_focus_init     (void);
   bool ov5640_focus_single   (void);
   bool ov5640_focus_constant (void);

Power MAX77654 Module Design
----------------------------
PMIC (Power management IC, External to MCU/SDK), Only used in :ref:`MK11 Board <mk11_hardware>`. Right now a little underpowered, we can withness this while running the code on the board, there is a 18 Sec delay after turning the power for every module. Occassionally boot failure is also noticed right now. PMIC is connected to MCU via I2C Interface.

There are at least 5 Different voltage outputs.

* 1.8V Power Rail: MCU and the Touch IC, Always on.

Auxillary Power Rails: All are being used to power other modules.

* 1.2V Power Rail
* 2.7V Power Rail
* 10V Power Rail
* LED Power Rail: Can be tured OFF.

Auxillary Power rails should be tured ON by MCU in a specific routine, which is noted in main.c.

Important files:

* max77654.c -> PMIC Interface API definitions.
* max77654.h -> PMIC Interface APIs.

Note: power.c -> power_management_init function is used to initilize the internal power management using NRF SDK APIs.

**Power Init API:**
:: 

   bool max77654_init (void);

**Power Control API:**
:: 

   bool max77654_rail_1v8sw_on (bool on);
   bool max77654_rail_2v7_on   (bool on);
   bool max77654_rail_1v2_on   (bool on);
   bool max77654_rail_10v_on   (bool on);
   bool max77654_rail_vled_on  (bool on);
   bool max77654_led_red_on    (bool on);
   bool max77654_led_green_on  (bool on);
   max77654_status max77654_charging_status (void);
   max77654_fault  max77654_faults_status   (void);
   bool max77654_set_charge_current (uint16_t current);
   bool max77654_set_charge_voltage (uint16_t voltage);
   bool max77654_set_current_limit  (uint16_t current);

OLED Module Design
------------------
Sony OLED, is a micro OLED (very small, smaller than a finger nail). Conned directly to the data path to FPGA. MCU is connected to OLED via SPI Interface. Configuration of OLED is done using SPI Interface.

After configuration is done by OLED, luminance should be configured (Setting the brightness).

Important Files:

* oled.c -> Control APIs are defined in this file.
* oled.h -> OLED Interface APIs.

**OLED Bootup APIs**
::

   void oled_pwr_sleep (void);
   void oled_pwr_wake  (void);

**OLED Control APIs**
::

   void oled_config             (void);
   void oled_config_burst       (void);
   bool oled_verify_config      (void);
   bool oled_verify_config_full (void);
   void oled_set_luminance (enum oled_luminance_t level);