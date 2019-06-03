=========================================
Desirable Configuration for UBlox NEO-M8U
=========================================


Receiver Configuration
----------------------

#. Save Ublox settings to NVRAM after cfg changes are made with UBX-CFG-CFG
#. Enable save-on-shutdown for saving battery-backed RAM data to flash memory
#. Send UBX-MGA-INI-TIME_UTC from the host at startup for faster boot

Concurrent GNSS
---------------

#. Atleast 4 tracking channels must be available for each major GNSS
#. Check if antenna is GPS only or supports other GNSS providers
#. Coldstart the GNSS after any change that disables an active GNSS as auxiliary data (e.g. UTC andIonospheric parameters) will not be updated

Navigation Configuration Settings 
---------------------------------

#. Enable Automotive mode in Platform settings of UBX-CFG-NAV5
#. Use Navigation input filters for setting appropriate 3D/2D fixes, satellite elevation fix, signal threshold in UBX-CFG-NAV5
#. Output filters: Check the gnssFixOK flag in the UBX-NAV-PVT orthe NMEA valid flag. Fixes not marked valid should not normally be used.
#. Course over Ground Low-pass Output Filter: Activate a course over ground low-pass filter when the speed is below 8m/s in UBX-CFG-ODO
#. Set appropriate distance threshold for the Static Hold Mode
#. Degraded Navigation: Navigation modes which use less than four Satellite Vehicles. Constant altitude is assumed to compensate for the missing fourth SV

Clocks and Time
---------------

#. Use UBX-NAV-TIMELS to process leap seconds

Multiple GNSS Assistance (MGA)
------------------------------

#. Estimated Position can be sent to receiver by UBX-MGA-INI-POS_XYZ or UBX-MGA-INI-POS_LLH
#. The current time can either be supplied using UBX-MGA-INI-TIME_UTC message and times referenced to some GNSS can be delivered with the UBX-MGA-INI-TIME_GNSS message.
#. Navigation Database: Instruct u-blox receivers to dump the current state of their internal navigation database with the UBX-MGA-DBD-POLL message; sending this information back to the receiver (e.g. after a period when the receiver was turned off) restores the databaseto its former state, and thus allows the receiver to restart rapidly.
#. AssistNow Online Service: AssistNow Online is u-blox' end-to-end Assisted GNSS (A-GNSS) solution for receivers that haveaccess to the Internet
#. [TBD]Configure AssistNow Autonomous and AssistNow Offline appropriately

Untethered Dead Reckoning
-------------------------

#. Improved navigation performance in GNSS-denied conditions: errors caused by multipath orweak signal conditions are mitigated though the aid brought by the IMU.
#. The INS bridges short GNSS gaps which might be caused by tunnels or parking garages


