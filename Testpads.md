The Nintendo Switch mainboard has a series of testpads on the front and
back, presumably used in factory test, diagnostics, and early board
bringup procedures.

## Raw Logic captures

These are reference materials, taken from poking at I/O on various
testpads. <https://github.com/hedgeberg/Switch-Logic-Captures>

## Photos

![Switchre\_side1.jpg](Switchre_side1.jpg "Switchre_side1.jpg")
![Switchre\_side2.jpg](Switchre_side2.jpg
"Switchre_side2.jpg")

## Pinouts

### Cluster A

| Pad \# | Name           | Type        | Levels | Continuity | Frequency               | Comment                                                      |
| ------ | -------------- | ----------- | ------ | ---------- | ----------------------- | ------------------------------------------------------------ |
| 1      | Batt GND?      |             |        |            |                         |                                                              |
| 2      | Battery pulse? | Pulse train | 0-3.3V | L-5?       |                         |                                                              |
| 3      | Battery Vdd    |             |        |            |                         |                                                              |
| 4      | ??             | Square wave | 0-3.3V |            | 329kHz? (undersampled?) | Square wave when screen on, but looks like vias to Speaker R |
| 5      | ??             | Square wave | 0-3.3V |            | 329kHz? (undersampled?) | Square wave when screen on, but looks like vias to Speaker R |
| 6      | Weak GND?      |             |        |            |                         |                                                              |
| 7      | SDA            | I2C         | 0-1.8V |            |                         |                                                              |
| 8      | SCL            | I2C         | 0-1.8V |            |                         |                                                              |
| 9      | USB-PWR-WAVE?  | Square wave | 0-3.3V | K-4, K-5?  | ~11 Hz                  |                                                              |
| 10     | USB-PWR-WAVE?  | Square wave | 0-3.3V | K-4, K-5?  | ~11 Hz                  |                                                              |

### Cluster C

| Pad \# | Name      | Type | Levels | Continuity | Frequency | Comment                                                                                                     |
| ------ | --------- | ---- | ------ | ---------- | --------- | ----------------------------------------------------------------------------------------------------------- |
| 1      | ??        |      | 0-1.8V |            |           | No clue. This is definitely important, we just have no idea how. May need to interface with dock for comms. |
| 2      | UART-A RX |      | 0-1.8V |            |           |                                                                                                             |
| 3      | UART-A TX |      | 0-1.8V |            |           |                                                                                                             |
| 4      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 5      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 6      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 7      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 8      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 9      | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 10     | ??        |      | 0-1.8V |            |           |                                                                                                             |
| 11     | ??        |      | 0-1.8V |            |           |                                                                                                             |

### Cluster E

| Pad \# | Name  | Type | Levels | Continuity | Frequency | Comment |
| ------ | ----- | ---- | ------ | ---------- | --------- | ------- |
| 10     | Reset |      |        |            |           |         |

### Cluster G

| Pad \# | Name         | Type | Levels | Continuity | Frequency | Comment   |
| ------ | ------------ | ---- | ------ | ---------- | --------- | --------- |
| 9      | BUTTON\_HOME |      |        |            |           | RCM strap |

### Cluster I

| Pad \# | Name       | Type         | Levels | Continuity | Frequency | Comment                                                         |
| ------ | ---------- | ------------ | ------ | ---------- | --------- | --------------------------------------------------------------- |
| 1      | GND        |              |        |            |           |                                                                 |
| 2      | Screen\_on | On/Off       | 0-1.8V |            |           | Screen power state, active high                                 |
| 3      |            | UART         | 0-1.8V |            | 1.5MBaud? |                                                                 |
| 4      |            | UART         | 0-1.8V |            | 1.5MBaud? |                                                                 |
| 5      |            | Flow control | 0-1.8V |            |           | Flow control for pad I-4?                                       |
| 6      |            |              | 0-1.8V |            |           | Needs testing with chip/touch screen interface board plugged in |

### Cluster J

| Pad \# | Name         | Type       | Levels | Continuity | Frequency | Comment                                                |
| ------ | ------------ | ---------- | ------ | ---------- | --------- | ------------------------------------------------------ |
| 1      | ?            | Edge       | 0-1.8V |            |           | Turns on around same time as pad J-3                   |
| 2      | GND          |            |        |            |           |                                                        |
| 3      | ?            | Edge       | 0-1.8V |            |           | Turns on around same time as pad J-1, slightly after   |
| 4      | Power button | Pushbutton | 4V-0V  |            |           | Active low                                             |
| 5      | ?            | Constant?  | 0V     | Ground?-NT |           |                                                        |
| 6      | ?            | Edge       | 0-1.8V |            |           | Turns on with pad J-6, ~1s after J-1/J-3               |
| 7      | ?            | Edge       | 0-1.8V |            |           | Turns on with pad J-5, ~1s after J-1/J-3               |
| 8      | ?            | Edge?      | 0-1.8V |            |           | Turns on ~1s after J-6/J-7, turns off at unknown point |

### Cluster K

| Pad \# | Name          | Type          | Levels  | Continuity | Frequency | Comment                                                                                                 |
| ------ | ------------- | ------------- | ------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------- |
| 1      | GND           |               |         |            |           |                                                                                                         |
| 2      | Unknown       | ??            | 3.3V-0V | None known | N/A?      | Falls around same time pad K-7 falls, but immediately. No data observed as of yet.                      |
| 3      | Unknown       |               |         |            |           |                                                                                                         |
| 4      | USB-PWR-WAVE? | Square wave   | 0V-3.3V | A-9, A-10? | ~11 Hz    |                                                                                                         |
| 5      | USB-PWR-WAVE? | Square wave   | 0V-3.3V | A-9, A-10? | ~11 Hz    | Appears to mirror K4. Duty cycle 66.67%. Low on screen lock. Off until first interaction.               |
| 6      | USB-C V+      | Supply power  |         |            |           |                                                                                                         |
| 7      | Unknown       | Power supply? | ~3V-0V  | None known | N/A       | 0 when usb-c not plugged in, falls slowly on first interaction if USB-C plugged in. Power draw related? |

### Cluster L

TODO: Update
diagram

| Pad \# | Name                   | Type          | Levels      | Continuity | Frequency | Comment                                                  |
| ------ | ---------------------- | ------------- | ----------- | ---------- | --------- | -------------------------------------------------------- |
| 1      | Li-Ion Batt Vdd Mirror | Power Supply  | Std. Li-Ion |            |           |                                                          |
| 2      | GND                    |               |             |            |           |                                                          |
| 3      | Li-Ion Batt Vdd        | Battery Input | Std. Li-Ion |            |           |                                                          |
| 4      | Mirrored Ground?       |               |             |            |           | Holds steady @ 0, looks like a decoupled isolated ground |
| 5      | Battery pulse?         |               |             |            | \<1 Hz    | Duty cycle ~0%                                           |