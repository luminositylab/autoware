# Steps:

* Figure out what info is needed for Autoware:
`gnss_localizer` transforms *pose info* from a [Message type](http://docs.ros.org/api/nmea_msgs/html/msg/Sentence.html) which belong to a ros package: [**NMEA_Sentence**](http://wiki.ros.org/nmea_msgs).
* Publish NMEA_Sentence in u-blox driver!
```
Node => GetParam (Config Ports) => 
```
All configurations stay same in the yaml file and parser stays the same!
Change Publish Message type.
* Config params: frequency and IMU.

* Enable Autoware with starting Ublox driver.



# Note
## 
* [Number Formats](https://www.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_(UBX-13003221)_Public.pdf#page=157) --  Variable Type Definitions.
* MonVER: The product type is determined from parsing the MonVER message.
* class **gps**: Handles communication with the U-Blox Devices.
* kSubscribeRate: Default subscribe Rate to u-blox messages [Hz].
* ublox_msgs/NavPOSLLH.msg is Navigation latitude, longitude and height messages.
* NavSOL message is for Firmware <=6.
* in node.cpp, the `fix_` variable is only for Firmware <= 6, the right var to use is `fix` which is advertised in *node.h* + ***Line 789***.



## Autoware Interfaces and message type


Autoware interfaces require GNSS message type to be [**NMEA_Sentence**](http://wiki.ros.org/nmea_msgs),(`nmea2tfpose_core.cpp` in `nmea2tfpose`) There are four sub-message types: `QQ``, $PASHR`, `$GPGGA`, and `$GPRMC`. They are all **nmea/sentences** but contains different information. \
`QQ` and `$PASHR` provides with `roll, pitch, and yaw`.\
Autoware parses `Latitude, Longitude, and Height` info from `$GPGGA` and `$GPRMC`.

### Msgs

* `QQ`: contains `time_stamp`, `roll`, `pitch` and `yaw`.
* `$PASHR`: [Inertial Attitude Data](https://docs.novatel.com/OEM7/Content/SPAN_Logs/PASHR.htm), contains `time_stamp`, `roll`, `pitch` and `yaw`.
* `$GPRMC`: [Recommended minimum specific GPS/Transit data](https://docs.novatel.com/OEM7/Content/Logs/GPRMC.htm?tocpath=Logs%7CView%20Logs%7CGNSS%20Logs%7C_____59), contains `time_stamp`, `lat`, `long` and `height`.
* `$GPGGA`: [Global Positioning System Fix Data](https://docs.novatel.com/OEM7/Content/Logs/GPGGA.htm), contains `time_stamp`, `lat`, `long` and `height`.

## Ublox Interfaces and message type (partial)

 [UBLOX MANUAL](https://www.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_%28UBX-13003221%29_Public.pdf#page=354&zoom=100,0,0)

`NAV`: Navigation Results Messages: Position, Speed, Time, Acceleration, Heading, DOP, SVs used.

`ESF`: External Sensor Fusion Messages: i.e. External Sensor Measurements and Status Information.

`ADR/UDR`: Automotive Dead Reckoning/Untethered Dead Reckoning.

## Abbrs

* `RTC` abbr for Real Time Clock.
* `DOP` abbr for Dilution of Precision: specify the additional multiplicative effect of navigation satellite geometry on positional measurement precision.
* `RTCM` abbr for Radio Technical Commission for Maritime Services.

**Msgs ID classes** from *P153/409* or *P138/394* in [manual](https://www.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_%28UBX-13003221%29_Public.pdf#page=354&zoom=100,0,0).
* `AID` abbr for AssistNow Aiding Messages.
* `CFG` abbr for Configuration Input Messages.
* `INF` abbr for Information Messages: i.e. Printf-Style Messages, with IDs such as Error, Warning, Notice.
* `NAV` abbr for Navigation Results Messages: Position, Speed, Time, Acceleration, Heading, DOP, SVs used.
* `ESF` abbr for External Sensor Fusion.
* `HNR` abbr for High Rate Navigation Results Messages: i.e. High rate time, position, speed, heading.
* `RXM` abbr for Receiver Manager Messages: Satellite Status, RTC Status.
* `TIM` abbr for Timing Messages: Time Pulse Output, Time Mark Results.

## Issues
* [**`$GNTXT`** shows when running `sudo cat /dev/ttyACM0`](https://raspberrypi.stackexchange.com/questions/88916/getting-nmea-unknown-msg58-and-other-garbage-from-my-neo-6m-0-001-gps-module). Partial Information can be found in [Firmware Manual](https://www.u-blox.com/sites/default/files/GNSS-FW3.01_ReleaseNotes_%28UBX-16000319%29_Public.pdf#page=9).
