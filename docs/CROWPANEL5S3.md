# Elecrow CrowPanel Basic 5.0 inch, ESP32-S3

This is the **Elecrow CrowPanel Basic 5.0-inch ESP32-S3 HMI display** (SKU DIS08050H): an 800 x 480 TFT LCD panel driven by ILI6122 and ILI5960 via parallel RGB interface (`rpi_dpi_rgb`), GT911 capacitive touch, and an ESP32-S3-WROOM-1 module with Flash and PSRAM.

Pages, tiles, swiping between pages, dimming, and waking by touch work on this panel.

## Which board

- **Hardware revision V1.0, V2.0, or V3.0.** Version 2.0 adds automatic downloading over Version 1.0 (eliminating the need to press boot and reset manually), and Version 3.0 adds touch timing control to prevent touch failures. The revision is printed on the back of the board.
- **ESP32-S3 module.** Built for the standard ESP32-S3 architecture (`board: esp32-s3-devkitc-1`), utilizing 8 MB PSRAM in octal mode.

## Install

Update Tessera Screen Manager and choose **CrowPanel · 5.0 inch, Basic 5.0 ESP32-S3** in **New screen**. Follow [Easy setup](EASY_SETUP.md) to create a profile with its own name, Wi-Fi references, and unique API/OTA keys. Choose the correct USB port, or download the firmware to flash with ESPHome Web from your own computer. Keep an existing working profile if the board is already installed. Tessera adds it to Home Assistant by itself once it is on your Wi-Fi; then add its tiles.

The remote package is `packages/crowpanel5s3.yaml` and the checkout entry `checkout/crowpanel5s3.yaml`. Each combines `packages/core.yaml` with the board file `packages/boards/crowpanel5s3.yaml`. The ESP32-S3 and its PSRAM configuration come from `packages/hardware/esp32s3.yaml`, with this board's pin definitions in the board file.

## Layout and capabilities

- 800 x 480 at ~187 ppi. A screen starts with four columns of five lying down, and three columns of six standing up; the editor's Grid button gives it another.
- The panel native resolution is 800 x 480 (landscape orientation).
- The backlight is controlled via a PWM pin (GPIO 2): brightness is a percentage, and standby, night mode, Sleep, Wake, and alert flashes are supported.
- The GT911 touch controller reports direct pixel coordinates without needing manual calibration.

## Hardware references

- [Elecrow's repository for this board](https://github.com/Elecrow-RD/CrowPanel-5.0-HMI-ESP32-Display-800x480) and Elecrow's ESPHome V3.0 configuration example.
- [ESPHome RPI DPI RGB](https://esphome.io/components/display/rpi_dpi_rgb/): the panel uses the parallel RGB display platform (`rpi_dpi_rgb`) configured for 800 x 480 resolution.
- The pin mapping (per official ESPHome V3.0 config):
  - **Display Data Pins (DE, VSYNC, HSYNC, PCLK):** DE GPIO40, VSYNC GPIO41, HSYNC GPIO42, PCLK GPIO39.
  - **RGB Color Lines:**
    - Red: R0 GPIO45, R1 GPIO48, R2 GPIO47, R3 GPIO21, R4 GPIO14
    - Green: G0 GPIO5, G1 GPIO6, G2 GPIO7, G3 GPIO15, G4 GPIO16, G5 GPIO4
    - Blue: B0 GPIO8, B1 GPIO3, B2 GPIO46, B3 GPIO9, B4 GPIO1
  - **Touch (GT911 I2C):** SDA GPIO19, SCL GPIO20, Interrupt GPIO18, Reset GPIO38.
  - **Backlight:** GPIO2 (PWM controlled).

## What to report while testing

1. The board's hardware revision (V1.0, V2.0, or V3.0), a successful boot, and appearance in Tessera.
2. Correct colors and a stable picture across several page changes, a restart from the screen's settings, an update over the air, and a cold start.
3. Physical taps near each corner, slider drags, and edge swipes. Touch that lands mirrored or swapped points at the touch transform (`TOUCH_SWAP_XY`, `TOUCH_MIRROR_X`, `TOUCH_MIRROR_Y`).
4. The picture standing up as well, built that way from New screen.
5. The brightness setting across its range, standby after the timeout and wake by touch, several times in a row, and night mode.
6. `sensor.<screen>_heap_free` with a full layout, any reset, Wi-Fi drop or I2C error, and how long the screen stayed running.

Do not publish Wi-Fi passwords or API/OTA keys with logs.