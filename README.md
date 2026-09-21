# Open firmware for the HU-058D WiFi clock with added buzzer, NTC and light sensor

I have added support for buzzer, temp and light sensor. The 5V needs to be cut and 3.3V connected instead. Only clock.yaml has been changed. Added temperature, beep with changeable duration and frequency and light (0-100%) to Home Assistant device dashboard. 

The new connections are:

| ESP32 | Socket pin | Net |
| --- | --- | --- |
| GPIO34 | 3 | RT, NTC temperature sensor |
| GPIO35 | 4 | RP, LDR light sensor |
| GPIO25 | 11 | RB, buzzer |


The NTC thermistor calibration can be adjusted in the `calibration` section of `clock.yaml`. I have not calibrated the sensor and it seems to be reading a little high on my device with the "default" values. 

| Setting | Default | Description |
| --- | --- | --- |
| `b_constant` | `3950` | Beta (B) value of the NTC thermistor |
| `reference_temperature` | `25°C` | Reference temperature |
| `reference_resistance` | `10kOhm` | Thermistor resistance at the reference temperature |

![ESP32 cut](docs/images/esp32%20cut.png)

![ESP32 3.3V](docs/images/esp32%203_3V.png)

I had a problem with OTA when I compiled the new clock.yaml so I recommend using USB the first time. The problem was that the old code was still running after firmware upload. Smaller changes have worked to program OTA after the initial change so I don't know why I had a problem the first time.

I made a test automation in Home Assistant to automatically adjust the display brightness. It works but it's just for demo.  

In the example code, an LDR value of 4% corresponds to 0% display brightness, while an LDR value of 30% corresponds to 50% display brightness, with linear scaling.
### Automatic Display Brightness

```yaml
alias: WiFi Clock - Automatic Display Brightness
description: ""
mode: restart

triggers:
  - trigger: state
    entity_id: sensor.wifi_clock_light

actions:
  - action: light.turn_on
    target:
      entity_id: light.wifi_clock_display
    data:
      brightness_pct: >
        {% set min_light = 3 %}
        {% set max_light = 20 %}
        {% set min_brightness = 1 %}
        {% set max_brightness = 50 %}
        {% set light = states('sensor.wifi_clock_light') | float %}
        {{ ((light - min_light) / (max_light - min_light) * (max_brightness - min_brightness) + min_brightness) | round }}
```

Home Assistant automation that plays Donkey Kong theme on the buzzer when button1 is pressed. Works better than expected.

### Donkey Kong theme
```yaml
alias: WiFi Clock - Donkey Kong theme
description: ""
triggers:
  - trigger: state
    entity_id: binary_sensor.wifi_clock_button_1
    from: "off"
    to: "on"
conditions: []
actions:
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 262
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 262
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 440
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1380
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 330
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 392
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 349
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 294
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2500
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1660
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 294
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 294
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 466
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1385
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 392
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 270
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 440
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 392
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 330
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2500
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1655
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 330
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 285
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 330
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 270
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 523
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1390
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 440
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 270
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 466
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 523
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 587
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2500
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1660
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 349
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 392
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 280
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 440
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 2000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1380
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 330
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 349
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 392
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1000
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 275
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_frequency
    data:
      value: 349
  - action: number.set_value
    target:
      entity_id: number.wifi_clock_buzzer_duration
    data:
      value: 1650
  - action: button.press
    target:
      entity_id: button.wifi_clock_buzzer_beep
  - delay:
      milliseconds: 1650
mode: restart
```





The AliExpress kit sold as an "ESP8266 IoT Colorful WiFi Clock Kit" ships closed firmware which is lame and terrible. (And really hard to use.) I have reverse engineered the way to drive the display so you can use your own microcontroller. 

I used an ESP32 running ESPHome so I have full Home Assistant integration and a clock that is far more useful than the $8 original form.

If you only want the reverse engineering, read `docs/`. You can drive this panel from any MCU you like.

![Every LED position on the HU-058D panel](docs/images/led-positions.png)

## The board variants

Same display and same 8051. The RTC boards also move one display clock wire.

| Board | Sold as | Timekeeping |
| --- | --- | --- |
| HU-058D | [ESP8266 IoT Colorful WiFi Clock Kit](https://www.aliexpress.us/item/3256807371597845.html) | ESP-01S, NTP over WiFi |
| HU-058 / HU-058SE | [DIY KIT Electronic Clock LED Microcontroller Soldering Exercise](https://www.aliexpress.us/item/3256806414008654.html) | Dallas RTC and a coin cell |

![The WiFi kit listing](docs/images/listing-esp8266.png)

![The Dallas RTC kit listing](docs/images/listing-dallas.png)

**An STC8G1K17 8051 MCU is the brain of both.** It sits in a DIP-16 socket and it
does everything to run the clock: scans the display, reads the buttons, reads a NTC for room temp, a light sensor and drives the buzzer.

Neither the ESP nor the Dallas chip ever touches the panel.

So despite the name, the WiFi one is not an ESP8266 clock. The ESP-01S is a
coprocessor that feeds time it gets from the internet to the 8051 over a serial link,
and its only connection to the rest of the board is one GPIO out and one GPIO
in. A DS1302 is used on the cheap board is for timekeeping, and you manually set it
through the terrible UI on the clock.

Buy whichever is cheaper. You are throwing away the timekeeping part either way.

The display is four seven-segment digits and a colon, built from 33
common-anode RGB LEDs behind two AiP33628 matrix drivers, one per two-wire
bus. The AiP33628 is a Wuxi I-CORE part with a Chinese-only datasheet, no
command set and no grayscale engine. It is a 30-bit shift register in front of
an output latch, so the scan, the brightness and the color mixing are all up to the driving MCU.

The LED display on this clock is multiplexed at around 400hz, so there may be some visible flicker. If you are sensitive to this, you should look elsewhere. It is possible firmware changes could drive the multiplex faster, reducing the flicker. I find the current multiplex rate totally accepetable so I haven't bothered to explore more.

## The conversion

Do not install the socket for the MCU, you will be connecting the new MCU to those pins. Do not install the Dallas or ESP device. Leave off the NTC and light sensor. Don't bother with the buzzer and driver transistor unless you want to use those. They are not used in my firmware. 

The buttons are connected to the ESP32-WROOM-32 in the ESPHome firmware, and are exposed as entities in Home Assistant. If your ESP32 board doesn't expose the GPIO lines listed below, you will need to change the firmware to match the hardware you have. The WROOM-32 board fits perfectly inside the existing clock case once your remote the pin headers.

| ESP32 | Socket pin | Net |
| --- | --- | --- |
| GPIO22 | 14 on HU-058D; 16 on HU-058 / HU-058SE | CLK, driver 1 |
| GPIO21 | 5 | DATA, driver 1 |
| GPIO19 | 1 | CLK_1, driver 2 |
| GPIO18 | 2 | DATA_1, driver 2 |
| GPIO32 | 9 | S1, top button |
| GPIO33 | 10 | S2, bottom button |
| GPIO34 | 3 | RT, NTC temperature sensor |
| GPIO35 | 4 | RP, LDR light sensor |
| GPIO25 | 11 | RB, buzzer |
| GND | 8 | GND |
The drivers run from the 5V rail but work perfectly with the 3.3V drive signals from the ESP32.

Step-by-step build notes are in `docs/wiring.md`.

## Documentation

| Path | What |
| --- | --- |
| `docs/wiring.md` | Board prep, socket wiring, power, case fit |
| `docs/hardware.md` | What is on the board, pinouts and variants |
| `docs/aip33628-protocol.md` | Frame format, latching, current levels |
| `docs/display-map.md` | COM and SEG to segment, scan, brightness, color |
| `docs/led-layout.md` | Physical coordinates for all 33 LED positions |
| `docs/home-assistant.md` | Every entity and action the firmware exposes |
| `reference/` | The AiP33628 datasheet |

The display map came out of logic analyzer captures of the stock firmware. 

## Firmware

`firmware/esphome/` is the firmware that runs the clock. An ESPHome external
component for the two drivers, plus a config that puts the panel in Home
Assistant:

- One RGB "light" entity for the whole display. Color sets the per channel duty,
  brightness sets the driver current.
- Effects: Per digit and per LED color, gradients at any angle, a color cycle and a
  flash effect.
- Switches for 12h/24h time and colon blink, the two front panel buttons as
  plain inputs, and actions to push a number/characters or the seconds onto the display
  for a few seconds. A unit of C or F lights the degree mark with the number.
- A lamp test button that lights every populated LED white for three seconds,
  then returns to the time with the previous settings.

`firmware/esp32/panel-test/` is a bare-metal PlatformIO project that drives
the same panel with nothing but the Arduino core. It is the better starting
point for a port, and its serial commands are the fastest way to find out
which LED positions a given board actually has fitted.

The firmware polls from the ESPHome NTP default servers every 15 minutes: 0.pool.ntp.org, 1.pool.ntp.org, 2.pool.ntp.org. The timezone comes from Home Assitant. 

## Warnings

The ESPHome firmware does not have any current limits. Driving the entire display at 100% (especially with all colors lit, like white) is not recommended for extended periods. I drive the clock around 45-50% and it seems to be very bright and runs cool. 

You are responsible for any damage you cause to your clock by modifying or using it. Be sensible about how you use it!

## Driving the panel from something else

Nothing about the panel needs an ESP32 or ESPHome. What you need is in three
documents:

1. `docs/aip33628-protocol.md` gives you the 30-bit frame and, more
   importantly, the latch rules. There is no strobe pin. The latch fires on a
   data edge while the clock line is high, which is a trap worth reading twice.
2. `docs/display-map.md` maps COM and SEG values to segments, and gives the
   scan rate and current levels.
3. `docs/led-layout.md` gives the physical coordinate of every LED, which is
   what a gradient or a wipe needs and the logical map does not carry.

The bit-banging is trivial. The two things that will cost you time are the
latch behavior and the fact that the current field is bit reversed against the
rest of the frame. Both are written up.

## Color resolution

The AiP33628 has no grayscale engine, so every duty cycle level costs a sub-frame.
The ESPHome component splits each COM slot into four binary weighted
sub-frames of 40, 80, 160 and 320us, which gives us 16 levels per channel and
4096 colors. Adjacent sub-frames carrying identical data merge before the
schedule reaches the scan interrupt, so a saturated color still costs one
frame per COM pair.

## Status

With the ESP32 firmware, the clock shows NTP time. All configuration is done with Home Assistant which owns color and brightness along with effects and settings like 12/24hz time, etc. 

Firmware updates go over the air, part of the ESPHome suite.

Out of scope by choice: the buzzer, the light sensor and the NTC thermistor. The
buttons are exposed to Home Assistant but carry no built-in behavior.

## AI Disclaimer

This project was coded with assistance from LLMs. Concept, design, testing, QA and code review was all done by me.

## License

Public domain, see `LICENSE`. The exception is code derived from ESPHome,
which stays under its own GPLv3.
