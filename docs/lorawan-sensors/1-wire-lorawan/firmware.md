# Multi Temperature Sensor Box

## Firmware

**Downloads:**

* [app-lorawan-onewire-bridge-0.5.1.hex](firmware/app-lorawan-onewire-bridge-0.5.1.hex)
* [app-lorawan-onewire-bridge-0.3.4.hex](firmware/app-lorawan-onewire-bridge-0.3.4.hex)

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
    
    Make sure to select the **"Firmware Updates"** checkbox!    

!!! warning "Changed Default Keys in v0.5.0"
    Starting with Firmware 0.5.0, the default values for `JoinEUI` (aka `AppEUI`), `AppKey`, and 
    `NwkKey` in the configuration have changed. 
    Be sure to check your LoRaWAN-Configuration between device and Network Server, after you 
    update to this version.
    
    You can adjust those values to your need with our config adapter. 

## Changelog
### 0.5.1 - 2020-10-15
#### Changed
- Set standard value for config parameter `LostReboot` to 1 (from 0).

### 0.5.0 - 2020-08-12
#### Added
- Power-On Selftest
#### Changed
- BREAKING CHANGE: Default values for `JoinEUI`, `AppKey`, `NwkKey` changed!
- Uses LoRaWAN-Stack that was certified for wMBus-Bridge
- Implement changes in LoRaWAN-Stack that happend during certification. Implements some 
edge-case.
- Make remote config feature robuster, e.g. when sending downlinks with ACK request.
#### Fixed
- Fixed Mixup of Sensors that could trigger endless loop

### 0.4.1 - 2020-07-21
#### Fixed
- Fix payload of sensor data

### 0.4.0 - 2020-06-30
#### Changed
- Use lobawan stack v1.2.2
- Sensors from `SensorIdOrder` that are missing will still be uploaded (with -0x8000 value)
- When `SendSensorId`==`false` uploads are done on port 3 (was port 2),
- Enable 3.3V step-up only if voltage is below 3.1V (was: if below 3.3V).
- Support up to 32 sensors (only 25 supported in `SensorIdOrder`).

### 0.3.3 - 2019-08-01
- Added ADR config parameter

### 0.3.2 - 2019-08-01
- Fixed Onewire init after DeepSleep

### 0.3.1 - 2019-07-29
- Temp sensors will be printed and sent sorted by parameter "SensorIdOrder" and by ID or just by ID if parameter "SensorIdOrder" is empty

### 0.2.0 - 2019-02-15
- Add parameter "SendSensorId" to allow skipping sensor IDs in payload

### 0.0.3
- Bugfixes in LoraWAN stack
- LoRaWAN Support for RX1 DataRate Offset

### 0.0.2
- Don't send sensor type with ID (support 6 sensors in 50 bytes)
- Add TestMode config value
- Add SendInternalTemp config value
- Internal sensor is send first if SendInternalTemp = true
