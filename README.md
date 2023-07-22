# Tools Used
Modify firmware on HC-42 or other nrf51xxx board with NRF5SDK and Segger Embedded Studio (SES)

Soft Tools used:
- Linux
- Segger Embedded Studio for ARM 6.34
- nRF5 SDK 17.10

Tools: 
- Segger Jlink+
- HC42 board

# Testing nrf5 sdk ble examples
1. Setup hc42 board
2. create directory in "nrfsdk5/examples/hc42-test"
3. put "hc42.h" in "nrfsdk5/components/boards/" directory
4. edit "nrfsdk5/components/boards/boards.h" to include "hc42.h", under custom_boards.h insert:
```
...
  #include "custom_board.h"
#elif defined(BOARD_HC42)
  #include "hc42.h"
#else
....
```
5. copy "nrfsdk5/examples/ble_peripheral/ble_app_blinky" to "nrfsdk5/examples/hc42-test/ble_app_blinky"
6. change directory to "nrfsdk5/examples/hc42-test/ble_app_blinky"
7. open "nrfsdk5/examples/hc42-test/ble_app_blinky/pca10040/s132/ses/ble_app_blinky_pca10040_s132.emProject"
8. right click "Project name" in the "Project Explorer" window, and choose "Option"
9. Change "Debug" to "Common" on the top left.
10. Choose "Code > Preprocessor > Preprocessor Definitions", click the triple dot
11. change from "BOARD_PCA10040" to "BOARD_HC42"
12. Click OK button

# Section placement setup
1. right click "Project name" in the "Project Explorer" window, and choose "Edit Section placement"
2. remove the "size="0x4" in <ProgramSection alignment="4" load="Yes" name=".text" size="0x4" />
3. remove the "size="0x4" in <ProgramSection alignment="4" load="Yes" name=".rodata" size="0x4" />
4. save and close the file

# SDK Config
1. edit "Application/sdk_config.h"
2. find "SoftDevice clock configuration" around line 11800th line.
3. change to: 
```
NRF_SDH_CLOCK_LF_SRC 0
NRF_SDH_CLOCK_LF_RC_CTIV 4
NRF_SDH_CLOCK_LF_RC_TEMP_CTIV 0
```
4. save and close the file

# Test Build
1. open main.c, and comment out all the "ADVERTISING_LED" and "CONNECTED_LED" occurence, since it is not defined.:
```
//#define ADVERTISING_LED                 BSP_BOARD_LED_0                         /**< Is on when device is advertising. */
//#define CONNECTED_LED                   BSP_BOARD_LED_1                         /**< Is on when device has connected. */
#define LEDBUTTON_LED                   BSP_BOARD_LED_0                         /**< LED to be toggled with the help of the LED Button Service. */
#define LEDBUTTON_BUTTON                BSP_BUTTON_0                            /**< Button that will trigger the notification event with the LED Button Service */
```
2. save main.c
3. press F7
4. some shortcuts:
- connect -- ctrl TC
- erase all -- ctrl TK
- download -- ctrl TL

