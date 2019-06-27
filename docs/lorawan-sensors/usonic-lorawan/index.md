# Ultrasonic LoRaWAN Sensor (a.k.a. "Oscar")

## Target Measurement / Purpose
Distance measurements via ultrasound.

**Features**

* 30cm to 3m detection range
* up to 8 detected objects

## Technical description

The device is based on the PGA460 from Texas Instruments ([http://www.ti.com/product/PGA460](http://www.ti.com/product/PGA460))

* [PGA460 Ultrasonic Module Hardware and Software Optimization](files/app_note.pdf){: target="_blank"}
* [PGA460 Ultrasonic Signal Processor and Transducer](files/pga460.pdf){: target="_blank"}

The PGA enables tuning of the Ultrasonic Sensor for all kind of environments. The default firmware supports parameters tested in underground waste bins.

Lobaro offers customized tuning and consulting for environments where the default configuration does not match.

### Open Top vs. Closed Top

As for now the sensor is shipped with an open top membran. This has advantages in sensibility and range, but is more prone to environmental impacts.

Closed-Top Sensors (e.g. as used in Automotive) need more energy to send out a clear signal but are resistant against environmental impacts. 
 

## Parser

### The Things Network (TTN)

This is a decoder written in JavaScript that can be used to parse the device's 
LoRaWAN messages. It can be used as is in 
[The Things Network](https://thethingsnetwork.org).

```javascript
function decodeUInt16(byte1, byte2) {
    var decoded = byte1 | byte2 << 8;
    if ((decoded & 1 << 15) > 0) {  // value is negative (16bit 2's complement)
        decoded = ((~decoded) & 0xffff) + 1;  // invert 16bits & add 1 => now positive value
        decoded = decoded * -1;
    }
    return decoded;
}


function Decoder(bytes, port) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var decoded = {};

    if (port === 2) { // Payload
        decoded.vBat = (bytes[0] | bytes[1] << 8) / 1000.0;  // byte 6-7 (originally in mV)

        decoded.temp = decodeUInt16(bytes[2], bytes[3]) / 10.0;
        decoded.numResults = bytes[4];
        var idx = 5;

        decoded.results = [];


        for (var i = 0; i < decoded.numResults; i++) {
            var result = {};

            result.distance_mm = bytes[idx] | bytes[idx + 1] << 8 | bytes[idx + 2] << 16 | bytes[idx + 3] << 24;
            result.distance_m = result.distance_mm / 1000;
            result.tof_us = bytes[idx + 4] | bytes[idx + 5] << 8;
            result.width = bytes[idx + 6];
            result.amplitude = bytes[idx + 7];
            decoded.results[i] = result;
            idx += 8;
        }
    }

    // example decoder for status packet by lobaro
    if (port === 1) { // status packet
        decoded.firmwareVersion = bytes[0] + "." + bytes[1] + "." + bytes[2];  // byte 0-3
        decoded.vBat = (bytes[4] | bytes[5] << 8) / 1000.0;  // byte 6-7 (originally in mV)
        decoded.temp = decodeUInt16(bytes[6], bytes[7]) / 10.0;  // byte 8-9 (originally in 10th degree C)
        decoded.msg = "Firmware Version: v" + decoded.firmwareVersion + " Battery: " + decoded.vBat + "V Temperature: " + decoded.temp + "°C";
    }

    return decoded;
}
```

**Example parser result**
```json
{
  "numResults": 1,
  "results": [
    {
      "amplitude": 215,
      "distance_m": 0.761,
      "distance_mm": 761,
      "tof_us": 4539,
      "width": 122
    }
  ],
  "temp": 21.8,
  "vBat": 2.779
}
```

You can think of the usonic signal as a *strength of signal* or *volume* over time

* **Amplitude** ranges from 0&ndash;255 and has no unit. It is highly influenced by 
  the internal amplification parameter.
  An amplitude of 100 or above is interpreted as reflected signal. Values below 100
  are dismissed as background noise.
* **ToF** is the *Time of Flight* measured in µs. From this and the speed of sound the 
  distance to a detected object is calculated.
* **Width** indicates how "wide" a detected signal is in time. That is the time in µs
  before the amplitude drops back below the threshold.

### Encoder

Used to update configuration on the device.

```javascript
function Encoder(object, port) {
    // Encode downlink messages sent as
    // object to an array or buffer of bytes.
    var bytes = [];

    string = object["string"] || "";

    for (var i = 0; i < string.length; ++i) {
        bytes.push(string.charCodeAt(i));
    }

    return bytes;
}
```