# esp32-cam-nvg
Stream camera feed to a display with an ESP32-CAM. Optimized for NVGs.

## Hardware
This code is meant to be used with ESP32-CAM, OV7725 camera and GC9A01 display.

For this MCU board, a USB to TTL converter is needed to connect the board to a PC via a virtual COM port, for example with an FTDI FT232RL chip.

- [ESP32-CAM](https://docs.ai-thinker.com/en/esp32-cam) is selected for its sufficient performance and convenient FPC camera connector.
- [OV7725 camera](https://cdn.sparkfun.com/datasheets/Sensors/LightImaging/OV7725.pdf) is selected for its low light sensitivity superior to the OV2640 that usually comes with the board.
- [GC9A01 display](https://www.waveshare.com/wiki/1.28inch_LCD_Module) is selected for the lack of any higher resolution compact display that connects via SPI interface (especially a round one - convenient for the NVG format).

## Installation
- Install [Arduino IDE](https://www.arduino.cc/en/software/).
- Using Tools -> Boards Manager, install [`esp32` by Espressif Systems](https://github.com/espressif/arduino-esp32).
> Note: There were issues with recognizing the camera hardware using the latest version of esp32. Confirmed compatible version is 20.0.17.
- Using Tools -> Manage Libraries, install [`TFT_eSPI` by Bodmer](https://github.com/Bodmer/TFT_eSPI).
- In the folder of the `TFT_eSPI` library, locate file `User_Setup.h` and edit it for the desired display by uncommenting relevant lines and commenting the rest. For GC9A01, the file provided in this repo may be used.
- Using Tools -> Board, find esp32 -> AI Thinker ESP32-CAM and select it.
- Wire the ESP32-CAM to the USB to TTL converter according to the table below.
- Connect the USB to TTL converter to the PC.
- Using Tools -> Port, select the correct COM port.
> Note: On Windows, you may find the COM port number by seeing what's appearing in the Device Manager when you plug in the converter.
- Click Upload ("arrow right" button). This will compile and upload the code to the ESP32-CAM.
- If the Output console is stuck on `Hard resetting via RTS pin...`, you may unplug the converter.
- Wire the ESP32-CAM to the camera and display according to the table below.
- Connect a 5V power source. The camera feed should now be displayed.
> Note: In case of issues, you may view the board's console output by leaving the RX and TX wires of the converter plugged in. The output should be visible in Tools -> Serial Monitor.

## Compatible 3D printed NVG housings
- [DPNVG mk3](https://www.thingiverse.com/thing:4975853)
- [MDNVG](https://www.thingiverse.com/thing:5221673)

## Notes
- The code is optimized for low light performance. Especially `s->set_gain_ctrl(s, 0)` turned out to be crucial not to freeze when there's sudden lighting changes.
- The image may be tweaked by many settings as listed [here](https://randomnerdtutorials.com/esp32-cam-ov2640-camera-settings/).
- Many ESP32-CAM demos use JPEG format. OV7725 does not support it, hence `config.pixel_format = PIXFORMAT_RGB565` is necessary.
- I tried to write the images by direct memory access and failed due to the size of the ESP32-CAM's internal RAM.
- I tried skipping pixels of the corners beyond the round display and there was no improvement in performance. 

## Upload Wiring
| FTDI | ESP32 |
| :-----: | :-----: |
| DTR | - |
| RX | UOT |
| TX | UnR |
| VCC | 5V |
| CTS | - |
| GND | GND |

**On the ESP32-CAM, add a jumper I00 to GND to enable upload.**

## Display Wiring
| GC9A01 | ESP32 |
| :-----: | :-----: |
| RST | I02 |
| CS | I015 |
| DC | I012 |
| SDA | I013 |
| SCL | I014 |
| GND | GND |
| VCC | 3V3 |
