# HC-42
modify firmware on HC-42 board with nrf5 sdk and ses

Soft Tools used:
- Linux
- Segger Embedded Studio for ARM 6.34
- nRF5 SDK 17.10

Tools: 
- Segger Jlink+
- HC42 board

# Testing nrf5 sdk ble examples
Setup hc42 board
create directory in "nrfsdk5/examples/hc42-test"
put "hc42.h" in "nrfsdk5/components/boards/" directory
edit "nrfsdk5/components/boards/boards.h" to include "hc42.h", under custom_boards.h insert:
...
  #include "custom_board.h"
#elif defined(BOARD_HC42)
  #include "hc42.h"
#else
....

copy "nrfsdk5/examples/ble_peripheral/ble_app_blinky" to "nrfsdk5/examples/hc42-test/ble_app_blinky"
change directory to "nrfsdk5/examples/hc42-test/ble_app_blinky"

open "nrfsdk5/examples/hc42-test/ble_app_blinky/pca10040/s132/ses/ble_app_blinky_pca10040_s132.emProject"
right click "Project name" in the "Project Explorer" window, and choose "Option"
Change "Debug" to "Common" on the top left.
Choose "Code > Preprocessor > Preprocessor Definitions", click the triple dot
change from "BOARD_PCA10040" to "BOARD_HC42"
Click OK button

Section placement setup
right click "Project name" in the "Project Explorer" window, and choose "Edit Section placement"
remove the "size="0x4" in <ProgramSection alignment="4" load="Yes" name=".text" size="0x4" />
remove the "size="0x4" in <ProgramSection alignment="4" load="Yes" name=".rodata" size="0x4" />

save and close the file

edit "Application/sdk_config.h"
find "SoftDevice clock configuration" around line 11800th line.
change to: 

NRF_SDH_CLOCK_LF_SRC 0
NRF_SDH_CLOCK_LF_RC_CTIV 4
NRF_SDH_CLOCK_LF_RC_TEMP_CTIV 0

save and close the file

open main.c:
//#define ADVERTISING_LED                 BSP_BOARD_LED_0                         /**< Is on when device is advertising. */
//#define CONNECTED_LED                   BSP_BOARD_LED_1                         /**< Is on when device has connected. */
#define LEDBUTTON_LED                   BSP_BOARD_LED_0                         /**< LED to be toggled with the help of the LED Button Service. */
#define LEDBUTTON_BUTTON                BSP_BUTTON_0                            /**< Button that will trigger the notification event with the LED Button Service */

and comment out all the "ADVERTISING_LED" and "CONNECTED_LED" occurence in main.c, since it is not defined.

save main.c

press F7
connect -- ctrl TC
erase all -- ctrl TK
download -- ctrl TL


