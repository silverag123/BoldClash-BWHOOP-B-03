[![Build Status](https://travis-ci.org/silver13/BoldClash-BWHOOP-B-03.svg?branch=master)](https://travis-ci.org/silver13/BoldClash-BWHOOP-B-03)

## SilverWare - BoldClash BWHOOP B-03 version

_Also works with B03 Pro_

Currently does not account for centered throttle! ( no altitude hold ) It uses normal throttle, low = motors off. That means it should be used with Devo / multimodule or another tx.

Programming connector is a Microjst 1.25 pitch connector. Don't wire positive to st-link as you may overcharge the battery if connected or damage the st-link.

Programming instructions ( uses St-link Utility program ):
https://www.rcgroups.com/forums/showthread.php?2876797-Boldclash-bwhoop-B-03-opensource-firmware

Compiling instructions ( uses Keil uVision IDE):
https://www.rcgroups.com/forums/showthread.php?2877480-Compilation-instructions-for-silverware#post37391059

### Level / acro mode change
By default "gestures" are used to change modes, move the stick *left-left-down* for acro mode , and *right-right-down* for level mode.

### Pid gestures and save
Pid gestures allow the pilot to change the acro mode pids by a percentage, and such tune the quadcopter without a computer. The new pids can be saved so that they will be restored after the quad is power cycled.The new pids will remain active if saved, until the pid values in file pid.c are changed. If the values are not changed, flashing the quad will not erase the pids unless the erase command is manually issued when programming.

*See also*
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=pidgesture

### Accel calibration / pid save
The gesture for accel calibration is down - down - down. If pids have been changed using the respective gestures since the last powerup or save, the pids will be saved instead.

### Telemetry
DeviationTx and multimodule+taranis can support telemetry, this requires no changes from the defaults on the quad. For devo, when selecting the Bayang protocol, hit Enter to see options, and enable telemetry there. Telemetry currently contains received number of data and telemetry packets, and 2 voltages, battery raw voltage and compensated voltage ( against voltage drop )

For multimodule, you need to add telemetry ( as an option ) to the bayang protocol in the protocol table ( configuration file ). Note the telemetry protocol won't work with stock quads.

*See also*
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=telemetry

### Android App telemetry
The Android app "SilverVISE" by SilverAG is able to receive telemetry from the quad using BLE packets. To use, the app protocol should be set in the quadcopter. The app will also show the pids.

*More information*
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=silvervise

### Android App telemetry and Devo telemetry with PIDs - switchable

This version of firmware contains dual telemetry mode, but only for bayang protocol. You can have Devo telemetry and SilverVISE together but activate only one at a time and switch between them using UUL gestures. Additionally, you can enable PID displaying on Devo telemetry screen and switch between PID and standard telemetry data using options in config.h and UUR gestures. For details check RC Groups forum post: https://www.rcgroups.com/forums/showpost.php?p=38611570&postcount=1290

### Dual PIDs

It is possible to enable dual PID mode and assign selection between two PID sets to desired channel. Both PID sets can be tuned separatelly via gestures, and can be saved with PID save gesture. Currently selected PID set will be displayed in Devo or in SilverVISE application. For details check RC Groups forum post: https://www.rcgroups.com/forums/showpost.php?p=39167380&postcount=1979

### Linux/gcc version
The gcc version compiles close to 16k, and may need turning off features in order to make it fit. It's also possible to flash up to 32K with changes. Read __install.md__ for additional information. 

The boldclash settings may need changes to work with gcc well. Turn off the software lpf filters except the last 2, the loop time is longer, at 1400, which affects the frequency of other soft lpf filters. You could set loop time to 1500 but it may not be needed.

### Wiki
http://sirdomsen.diskstation.me/dokuwiki/doku.php?id=start


-------------------
Fork changes:

### 04.03.18
+ Added factory radio settings to have better BLE signal for SilverVISE (based on silverxxx changes from 04.03.18)

### 13.02.18
+ Dual PIDs support

### 19.11.17
+ Dual telemetry - switch between SilverVISE and Devo telemetrry with gestures
+ PIDs shown at Devo screen (partial solution - helpfull but not 100% usefull)

-------------------
Official FW changes (source for this fork):

### 29.08.17
* moved flash save to 31k
* added 2 looptime independent soft lpf
* added lowerthrottlemix3 from h8
* added motor filter 2 replacing original,much better performance
* pid save has own led flash now
* cleanup of unused features: headless and some other


### xx.06.17
* pid gestures - pids can be changed by gestures at the field
* pid save - new pids are saved to flash by the d-d-d gesture

### 02.05.17
* level pid is tuned more aggressive
* added filter between level pid and  acro pid
* fixed issue where "Overclock" option broke level mode (i2c) - overclock can be used on the bwhoop now
* removed level mode I term - this was not usually used
* minor cleanup and efficiency
* GCC autovdrop now functional
* LVC throttle lowering disabled by default just in case it malfunctions

### initial changes

* autovdrop improved, and better compensation
* new measured motor curve at 24K pwm
* added a new way of lowering throttle below a certain voltage
* lvc now flashes below an "uncompensated" voltage, too, just in case
