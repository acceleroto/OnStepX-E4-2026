OnStepX Telescope Controller
===========================

# This fork
This fork tracks current OnStepX `main` but is configured as a FYSETC E4 v1.0 controller for a Losmandy G11 GEM with 0.9 deg/step stepper motors and a 3:1 pulley reduction.

Compared with upstream `main`, this fork:
* Enables the `FYSETC_E4` pinmap.
* Configures RA and DEC as TMC2209 step/dir axes using the newer `TMCStepper` backend.
* Sets the G11 drivetrain geometry to 38400 steps/degree and 38400 PEC steps/worm rotation.
* Enables ESP32 WiFi access point mode and the built-in website plugin.
* Enables BME280 weather sensing at I2C address `0x76`.
* Enables a DS3231 RTC as the time/location source.
* Adds two TMC2209 focusers on axes 4 and 5.
* Adds two manual dew heater outputs on FYSETC E4 heater pins 2 and 4.
* Keeps driver status polling disabled for RA/DEC because TMC readback currently reports comms faults on this E4 setup.

Compared with upstream `E4`, this fork:
* Uses current OnStepX `main` as the base instead of the older E4 branch release.
* Treats the E4 branch as a hardware reference, not as the final software authority.
* Uses `DRIVER_TMC_STEPPER` for the FYSETC E4 TMC UART drivers instead of the legacy `TMC2209.h` driver path.
* Keeps the current mainline FYSETC E4 UART and limit pin defaults rather than carrying over the old `SERIAL_TMC_RX 35` and `LIMIT_SENSE_PIN 34` overrides.
* Includes the current website plugin source in this fork so the onboard ESP32 serves the web interface directly.
* Documents the E4/G11 port notes in `docs/e4-main-port-notes.md`.

# What is OnStepX?
OnStepX is the advanced version of the OnStep computerized telescope controller with support for interfacing with/controlling a variety of motor drivers (and so motors) including Step/Dir, ODrive, and Servo (a combination of encoder and DC motor or Stepper motor) types.

It supports:
* Telescope Mount control (Alt/Azm and Equatorial GEM/Fork.)  Optional support for Eq mounts with Tangent Arm Declination.  Usually the Goto capability is enabled, but that's optional as well for those who just want basic mount control.
* Telescope Rotator control (including Alt/Azm de-rotation.)
* Telescope Focuser control (up to 6 focusers so it can handle collimation as well as focusing.)
* Telescope Accessory control (combination of up to 8 dew-heaters, switches, analog PWM.)

# Features
OnStepX supports a wide variety of connection options.  Several serial
"command channels" can be utilized. One of the these is normally devoted to a USB
connection and for the other(s) choose from the following:

* Bluetooth
* ESP8266 WiFi
* Arduino M0/Ethernet Shield
* Even another USB port or RS232 serial isn't very difficult to add.

In the case of running OnStepX on an ESP32 it can provide its own Bluetooth or WiFi IP command channels without additional hardware by simply activating the feature in OnStepX.

Other software in the OnStep ecosystem include:

* an [ASCOM](http://ascom-standards.org/) driver (with IP and Serial support),
* an Android App useable over WiFi or Bluetooth equipped Phones/Tablets,
* a "built-in" website (on the Ethernet and/or WiFi device),
* a full planetarium program that controls all features ([Sky Planetarium](http://stellarjourney.com/index.php?r=site/software_sky)).

OnStep is compatible with the LX200 protocol. This means it can be controlled
from other planetarium software, like: Sky Safari, CdC (even without ASCOM),
Stellarium, etc.

There are also [INDI](http://www.indilib.org/about.html) drivers so it can be used from Linux, with CdC or KStars.

# Documentation
Detailed documentation, including the full set of features, detailed designs for
PCBs, instructions on how to build a controller, how to configure the firmware
for your particular mount, can all be found the [OnStep Group Wiki](https://groups.io/g/onstep/wiki/home).

# Change Log
All the changes are tracking in git, and a detailed list can be accessed using the
following git command:
 
git log --date=short --pretty=format:"%h %ad %<(20)%an %<(150,trunc)%s"

# Support
Questions and discussion should be on the mailing list (also accessible via the
web) at the [OnStep Group](https://groups.io/g/onstep/).

# License
OnStep is open source free software, licensed under the GPL.

See [LICENSE.txt](./LICENSE.txt) file.

# Author
[Howard Dutton](http://www.stellarjourney.com)

