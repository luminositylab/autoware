# COMPUTING

## Perception

### Localization

#### gnss_localizer
* In `nmea2tfpose`, initialize the gnss variable `geo_pos_conv` parameter with rough `lat&lon` values.
* The [`nmea_msgs/Sentence`](http://docs.ros.org/melodic/api/nmea_msgs/html/msg/Sentence.html) contains:
<pre>
# A message representing a single NMEA0183 sentence.

# header.stamp is the <a href="http://wiki.ros.org/roscpp/Overview/Time">ROS Time</a> when the sentence was read.
# header.frame_id is the frame of reference reported by the satellite
#        receiver, usually the location of the antenna.  This is a
#        Euclidean frame relative to the vehicle, not a reference
#        ellipsoid.
<b>std_msgs/Header <i><a href="http://docs.ros.org/melodic/api/std_msgs/html/msg/Header.html">header</a></b></i>
# <i>This should only contain ASCII characters in order to be a valid NMEA0183 sentence.</i>
<b>string <i>sentence</i></b>
</pre>
* While [`sensor_msgs/NavSatFix`](http://docs.ros.org/melodic/api/sensor_msgs/html/msg/NavSatFix.html) contains:
<pre>
uint8 COVARIANCE_TYPE_UNKNOWN=0
uint8 COVARIANCE_TYPE_APPROXIMATED=1
uint8 COVARIANCE_TYPE_DIAGONAL_KNOWN=2
uint8 COVARIANCE_TYPE_KNOWN=3
std_msgs/Header <b><i>header</i></b>
sensor_msgs/NavSatStatus status
float64 <b><i>latitude</i></b>
float64 <b><i>longitude</i></b>
float64 <b><i>altitude</i></b>
float64[9] position_covariance
uint8 position_covariance_type
</pre>

* The `ublox_msgs/NavATT.h` contains:
<pre>
# NAV-ATT (0x01 0x05)
# Attitude Solution
#
# This message outputs the attitude solution as roll, pitch and heading angles.
# Supported on ADR and UDR products.
#
uint8 CLASS_ID = 1
uint8 MESSAGE_ID = 5
uint32 <b><i>iTOW</i></b>               # GPS time of week of the navigation epoch [ms]
uint8 version             # Message version (0 for this version)
uint8[3] reserved0        # Reserved
int32 <b><i>roll</i></b>                # Vehicle roll. [deg / 1e-5]
int32 <b><i>pitch</i></b>               # Vehicle pitch. [deg / 1e-5]
int32 <b><i>heading</i></b>             # Vehicle heading. [deg / 1e-5]
uint32 accRoll            # Vehicle roll accuracy (if null, roll angle is not 
                          # available). [deg / 1e-5]
uint32 accPitch           # Vehicle pitch accuracy (if null, pitch angle is not 
                          # available). [deg / 1e-5]
uint32 accHeading         # Vehicle heading accuracy [deg / 1e-5]
</pre>
* [**iTOW** Timestamps](https://www.u-blox.com/sites/default/files/products/documents/u-blox8-M8_ReceiverDescrProtSpec_%28UBX-13003221%29_Public.pdf#page=40&zoom=100,0,0): 

All the main UBX-NAV messages (and some other messages) contain an iTOW field which
indicates the GPS time at which the navigation epoch occurred. Messages with the same iTOW
value can be assumed to have come from the same navigation solution.
Note that iTOW values may not be valid (i.e. they may have been generated with insufficient
conversion data) and therefore it is **not recommended** to use the iTOW field for any **other purpose**.

* So in `nmea2tfpose_core.cpp`, **fn** `callbackFromNAVATT` doesn't implement **time check**.
* **`position_time_`** variable in `nmea2tfpose_core.h` is parsed from `nmea_msgs/sentence` not from `header`. However, both `NavSatFix` and `NavATT` don't have **in-sentence time**.
