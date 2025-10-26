Project: Air Purifier Firmware

Platform: Arduino-based (ATmega32A or compatible)
Purpose: Control and monitor a smart air purifier with automatic fan speed control, AQI visualization, and persistent operating modes.

1. System Architecture
   Component Function
   PMS5003 Sensor Measures PM2.5 / PM10 concentrations to calculate AQI.
   PCF8574 I/O Expander Controls fan relays or triacs via digital outputs P5 and P7.
   Display (Adafruit_GFX / TFT / OLED) Shows AQI, timer, and mode indicators.
   FastLED RGB Ring Visualizes air quality with color transitions and optional breathing effect.
   EEPROM Stores system state (Power, Mode, Fan Speed) for recovery after power loss.
   Buttons User input for Power, Speed, Auto, Timer, and Sleep functions.

2. Operating Modes
   Mode Description
   Power OFF All outputs disabled, display blank, fan off.
   Manual Mode User sets fan speed (1–3).
   Auto Mode Fan speed adjusts automatically based on AQI thresholds.
   Sleep Mode LEDs and display off; fan maintains current setting.
   Timer Mode Runs for a set number of hours, then shuts down.
3. EEPROM-Persistent States
   Stored Variable Purpose
   PowerState Remembers ON/OFF after power loss.
   Mode Auto or Manual mode restored.
   FanSpeed Saved only in Manual mode.

Functions:

void saveStateToEEPROM(uint8_t mode, uint8_t speed);
void restoreStateFromEEPROM();

Saved on every mode or power change, restored during setup().

4. Main Functional Blocks
   4.1. setup()

Initializes serial communication, display, LEDs, and sensors.

Restores system state from EEPROM.

Configures GPIOs and I/O expander.

Displays boot animation or startup logo.

    4.2. loop()

Main real-time control cycle:

Reads AQI and current sensor.

Updates display and LED color.

Adjusts fan speed automatically if isAuto == true.

Handles button input events for power, mode, timer, and sleep.

Writes state changes to EEPROM.

(See docs/loop_function.md for full breakdown.)

    4.3. calculateAQI()

Maps PM2.5 concentration to standard AQI levels and returns an integer value used for color mapping and fan control.

    4.4. getFadeColor(int aqi)

Returns a CRGB color based on AQI band.

    4.5. set_FAN_speed(uint8_t speed)

Controls PCF8574 outputs to select low, medium, or high speed.

    4.6. onDisp() / offDisp()

Updates the display according to the active state.

    4.7. handleFade() and startFadeToColor()

Implements smooth LED transitions for AQI visualization.

    4.8. dispSegment(int value)

Displays timer or numeric data on the screen.

    4.9. play_device_on() / play_device_off() / play_menu_up() / play_menu_down()

Optional sound feedback via DFPlayer or buzzer.

5. AQI and Fan Control Logic
   AQI Range LED Color Fan Speed (Auto Mode)
   0–5 Blue 0 (Off)
   5–25 Green 1
   25–50 Yellow 1
   50–75 Orange 2
   75–100 Red 3
   100+ Purple 3
   
6. Current Sensor / Filter Check

Samples analog current from fan circuit (PIN_CURRENT_SENSE).

Averages multiple samples for stability.

Sets flag_change_filter when load exceeds CHANGE_HEPA_FILTER.

Uses hysteresis to prevent false positives.

7. Sleep Mode Behavior

Mode 1: LEDs off, fan runs.

Mode 2: LEDs + display off, fan off.

Mode 3 (default): Normal mode restored.

8. Safety and Reliability

EEPROM writes throttled to state changes to prevent wear.

Uses non-blocking timing (via millis()).

All outputs reset to safe states on startup and shutdown.
