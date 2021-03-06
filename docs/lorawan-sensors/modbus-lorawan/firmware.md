# Modbus LoRaWAN Bridge

## Firmware

!!! note "Make sure to use the latest version of the Lobaro Maintenance Tool!"
    Use at least version 1.4.9 of the [Lobaro Maintenance Tool](/tools/lobaro-tool.html) for 
    installing this firmware.

**Downloads:**

* [app-modbus-lora-bridge-1.2.2.hex](firmware/app-modbus-lora-bridge-1.2.2.hex). [current release]
* [app-modbus-lora-bridge-1.2.1.hex](firmware/app-modbus-lora-bridge-1.2.1.hex). 
* [app-modbus-lora-bridge-1.1.1.hex](firmware/app-modbus-lora-bridge-1.1.1.hex).
* [app-modbus-lora-bridge-1.0.3.hex](firmware/app-modbus-lora-bridge-1.0.3.hex).
* [app-modbus-lora-bridge-1.0.2.hex](firmware/app-modbus-lora-bridge-1.0.2.hex).
* [app-modbus-lora-bridge-1.0.1.hex](firmware/app-modbus-lora-bridge-1.0.1.hex). 
* [app-modbus-lora-bridge-0.3.1.hex](firmware/app-modbus-lora-bridge-0.3.1.hex). [latest release with old stack version]

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
    
    Make sure to select the **"Firmware Updates"** checkbox!
    


## Changelog

### 1.2.2 - 2020-06-18 - [current release]
#### Changed
- Using Lobawan 1.2.2 (fixes some issues with OTAA Joins)

### 1.2.1 - 2020-02-17
**Added**

- Display Version of Lobaro LoRaWAN Stack on boot.

**Fixed**

- Fix a crash when using remote command `append`.

### 1.2.0
**Added**

- New config parameter `MbAttempts` to control how often Modbus Commands will be repeated in case of timeouts.

### 1.1.1
**Changed**

- Increased size available for config.

### 1.1.0
**Added**

- New optional Listen-Before-Talk Modbus communication, so device can coordinate with another master on bus.

**Fixed**

- Fix issue that could cut of long Modbus responses.

### 1.0.3
**Changed**

- Dialog Mode now logs activity on RS485 Bus while not communicating (to detect other Modbus Masters).
- Log Frequencies for LoRaWAN.

### 1.0.2
**Added**

- Waitcycles for optional capacitors to load and stabilize on startup

### 1.0.1
**Changed**

- Using FRAM for storing results before uploading, allowing for multiple kB of data to be sent (size depending on hardware).

**Fixed**

- Removed memory corruption error that could be triggered by configurations with lots of Modbus commands.

### 1.0.0
**Added**

- LoRaWAN 1.1 support
- Remote configuration via LoRaWAN on port 128.
- Clock synchronisation via LoRaWAN.

**Changed**

- Random delay before Uplink (to prevent persistent collisions when using multiple devices).
- Modbus responses longer than payload now get split up (additional parts on port 5).

### 0.4.1
**Fixed**

- Changed error indication bit on error 11 from `0xf0` to `0x80`.
- Fixed issue when parsing multiple Modbus commands from config.

### 0.4.0
**Added**

- Writing values to holding registers and coils.
- Execution of arbitrary Modbus commands triggered by LoRaWAN Downlink messages.
- Support for LoRaWAN Operation Mode Class C (for short reaction time to Downlinks).
- Automated register writing and broadcasts possible through new configuration.

**Changed**

- Automated reading (triggered by cron) is now configured by entering actual Modbus commands (more flexibility and usage of already existing Modbus syntax &ndash; *this breaks old configurations*).
- Upload format changed to sending raw response to Modbus commands (*this breaks existing integrations*).

**Fixed**

- Flushing to avoid invalid byte received from switching from TX to RX.
- Modbus mode ASCII now counts received bytes correctly.
- DataLength of 7 bits can now correctly be set in config again.

### 0.3.1 &ndash; 2019-05-24
**Fixed**

- Increased robustness of data reception on higher Baud rates.
  
### 0.3.0 &ndash; 2019-05-15
**Added**

- Initial release of Firmware for new Hardware revision (with RS485-addon).
- Update Modbus to support all 4 types of registers.

**Changed**

- Parity bit must not be substracted from Data bits anymore. `8E1` can now be confiured with `8 Data bits, EVEN parity, 1 Stop bit`.


### 0.1.0 &ndash; 2018-08-13
**Added**

- Original hardware release (with RS-485 on holding PCB).
