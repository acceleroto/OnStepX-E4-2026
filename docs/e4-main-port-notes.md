# FYSETC E4 G11 Main Port Notes

Date: 2026-06-17

Base commit:

```text
f43dd51df76e5ea9689eb329f24ee480b9f7cb11
f43dd51d Updated gitignore for my local build environment
```

## Build Target

- Board: FYSETC E4 v1.0, ESP32.
- Mount: Losmandy G11 GEM.
- Worm reduction: 360:1.
- Belt reduction: 20T motor pulley to 60T worm pulley, 3:1.
- Motors: 0.9 degree steppers, 400 full steps/rev.
- Tracking microsteps: 32.
- Computed axis steps/degree: 38400.
- Computed PEC steps/worm rotation: 38400.

## Configuration Decisions

- Use current `main` as the base and set `PINMAP FYSETC_E4`.
- Keep `HOST_NAME "OnStep"` to match the existing E4 build convention.
- Use current `src/pinmaps/Pins.FYSETC_E4.h` pin definitions for TMC UART and limit sense.
- Do not carry over the old E4 branch `LIMIT_SENSE_PIN 34` or `SERIAL_TMC_RX 35` overrides.
- Enable WiFi access point mode with `SERIAL_RADIO WIFI_ACCESS_POINT`.
- Enable the OnStepX website plugin in `src/plugins/Plugins.config.h`.
- Enable website weather display and internal MCU temperature display.
- Enable BME280 at I2C address `0x76`.
- Enable DS3231 as the time/location source.
- Enable RA/DEC as TMC2209 step/dir axes.
- Enable two TMC2209 focusers on axes 4 and 5.
- Enable two manual dew heater outputs on pins 2 and 4.
- Disable thermistor-based focuser/dew temperature.
- Enable limit sense and prioritize the current FYSETC E4 limit input.
- Disable buzzer support.

## Arduino IDE Settings

Use the FYSETC E4 wiki guidance unless compiler output requires a change:

- Board: `ESP32 Dev Module`.
- ESP32 Arduino core: `2.0.17` recommended.
- CPU Frequency: `240MHz`.
- Partition Scheme: `Huge App`.
- Port: FYSETC E4 USB serial port.
- First major upload: `Erase All Flash Before Sketch Upload: Enabled`.

## Expected Libraries

Install as compiler errors identify them. Likely required:

- Adafruit BME280.
- Adafruit Unified Sensor.
- Makuna Rtc.
- TimeLib.
- TMCStepper or the current TMC library required by this OnStepX checkout.

## Website Notes

- The website runs on the FYSETC E4 onboard ESP32; no separate Smart Web Server board is required.
- Expected AP SSID comes from `HOST_NAME`, currently `OnStep`.
- Default AP password is `password`.
- Default website URL is `http://192.168.0.1/`.
- IP command channels are expected on ports `9996` through `9999`.

## Validation Notes

Compile verification is still required in Arduino IDE.

Before connecting the mount:

- Test motors unloaded.
- Confirm RA/DEC directions and adjust `AXIS1_REVERSE` or `AXIS2_REVERSE` if needed.
- Confirm no stalls at 2.5 deg/sec before testing higher slew rates.
- Confirm BME280 and DS3231 are detected.
- Confirm the website loads and shows weather plus internal MCU temperature.
- Confirm both dew heater outputs switch correctly before connecting heater straps.
- Confirm both focusers move unloaded before connecting to telescope hardware.
- Confirm limit input behavior before relying on it with the mount attached.

## NV/Flash Notes

If settings appear stale after upload:

1. Prefer Arduino IDE `Erase All Flash Before Sketch Upload: Enabled` for the first upload.
2. If runtime settings still appear stale, use the documented `NV_WIPE` procedure in `Extended.config.h`.
3. Never leave `NV_WIPE ON` permanently.
