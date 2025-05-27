# MAY 24, 2025

### GOALS
I thought of what I wanted to accomplish with this project, and made a quick list of all the capabilities I want for it:
- Bluetooth capabilities, it should be able to connect to airpods or headphones
- Headphone jack, if people want to use it
- A microSD slot, so that you can load songs onto the microSD and quickly listen to music
- A music player software such as mediamonkey that can manage the songs
- 4 buttons all around a fifth button in the middle, similar to the format of an ipod, with the 4 around being navigation and the fourth being confirmation
- A USB-c port for charging and connection to another device that can also load music onto the microSD card
- A LCD display
- Li-on battery
- 3D printed case
- Power button

CONCEPT:

![NeoPod-concept](https://github.com/user-attachments/assets/f404d724-f5f2-41ae-bf1e-169ad44bc721)
### PARTS AND POSSIBLE ARCHITECTURE
- So, now that I know what I want to accomplish, I should think of what components to use. After doing some research, I realized that an A2DP source is needed to send music over bluetooth to airpods, which an ESP32-WROOM32 would be perfect for. Paired with the [ESP-A2DP library](https://github.com/pschatzmann/ESP32-A2DP), I will have a fully functional A2DP sink. 
- Now onto the DAC, I will use a PCM5102 DAC and SJ-3523-SMT-TR barreljack. 
- The microSD slot will just be standard chassis. 
- For the 5 buttons, I might go for mechanical keys and keycaps with up, down, right, left arrows. I will use the Kailh Choc White V1 Low Profile Switches
- The USB-C connector will be a 16 + 8 receptacle (USB4115-GF-A) that can transfer data and charge, which I will use a STC4054GR for.
- If I want a method to debug over USB-C from my computer, I can use a CP2102 that is between the D+ and D- lines to convert USB to UART and send data over serial and charge the device. 
- For file transfer to the microSD, I will use a separate USB-C receptacle, a RP2040 with a 12MHz XTAL with 2 22pF capacitors, and a 4x multiplexer(SN74CB3Q3257DBQR) that switches access of the microSD between the rp2040 and the ESP32-WROOM when the cable is plugged in via sel pins
- I will use a 2" to 2.0" TFT LCD (ILI9341) that I can use SPI with
- After some research, a 3000 mAh 3.7v li-on battery should be sufficient to run the NeoPod for a while
- Add a momentary switch on the side to act as a [power button](https://randomnerdtutorials.com/esp32-deep-sleep-arduino-ide-wake-up-sources/), it will be connected to gpio0 of the ESP-WROOM so that it goes into and out of deep-sleep on press, use EXT0 on GPIO0

### KICAD SKETCH

I sadded the ESP32-WROOM-32, PCM5102a, SJ-3523-SMT-TR, and the SD slot. Now I am trying to find a symbol for the Kailh Choc V1.

![NeoPod_5/24/25](https://github.com/lsyzg/NeoPod/blob/main/Images/NeoPod_5-24-25.pdf)

DATASHEETS AND REPOSITORIES I AM USING NOW
- [GY-PCM5102a](https://todbot.com/blog/wp-content/uploads/2023/05/macsbug_pcm5102_info.jpg)
- [PCM5102a](https://www.ti.com/lit/ds/symlink/pcm5102a.pdf?ts=1748075100790&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FPCM5102A%253Futm_source%253Dgoogle%2526utm_medium%253Dcpc%2526utm_campaign%253Dasc-null-null-44700045336317125_prodfolderdynamic-cpc-pf-google-ww_en_int%2526utm_content%253Dprodfolddynamic%2526ds_k%253DDYNAMIC+SEARCH+ADS%2526DCM%253Dyes%2526gad_source%253D1%2526gad_campaignid%253D6465330681%2526gbraid%253D0AAAAAC068F3y38dTNUFxfDZH98xuF_1oU%2526gclid%253DCjwKCAjw3MXBBhAzEiwA0vLXQfC7bXXGllGd-9q7n_Mi0rRSxtiPGLOc2QiR0GRdfrctm7GgXN592xoCbpAQAvD_BwE%2526gclsrc%253Daw.ds)
- [ESP32-WROOM32](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf)
- [kiswitch - KiCad Keyswitch Footprints](https://github.com/kiswitch/kiswitch/tree/main)
  
TIME SPENT: 6hrs

# MAY 25, 2025

### KICAD SKETCH

I've researched how switches are connected in keyboards and have connected them to one GPIO each. I have also added two USB-C receptacles for charging, serial debugging, and data transfer. The RP2040 is added. For the screen, I will use a 2.2" TFT ILI9341. Added the STC4054GR and a linear regulator XC6210B332MR, wired the switches together, wired everything together. I am now down with the schematic, assuming none of the wiring is wrong. YAY

![NeoPod_5/25/25](https://github.com/lsyzg/NeoPod/blob/main/Images/NeoPod_5-25-25.pdf)

DATASHEETS AND REPOSITORIES I AM USING NOW
- [RP2040 Pinout](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html)
- [Another RP2040 Pinout](https://www.circuitstate.com/pinouts/raspberry-pi-pico-rp2040-microcontroller-board-pinout-diagrams/)
- [Audiojack](https://www.sameskydevices.com/product/resource/sj-352x-smt.pdf)
- [scottokeebs kicad keyboard libraries](https://github.com/joe-scotto/scottokeebs/blob/main/Extras/ScottoKicad/footprints/ScottoKeebs_Choc.pretty/Choc_V1.kicad_mod)
- [SN74CB3Q3257DBQR Datasheet](https://www.ti.com/lit/ds/symlink/sn74cb3q3257.pdf?HQS=dis-dk-null-digikeymode-dsf-pf-null-wwe&ts=1748192144325&ref_url=https%253A%252F%252Fwww.ti.com%252Fgeneral%252Fdocs%252Fsuppproductinfo.tsp%253FdistId%253D10%2526gotoUrl%253Dhttps%253A%252F%252Fwww.ti.com%252Flit%252Fgpn%252Fsn74cb3q3257)

TIME SPENT: 6hrs

# MAY 26, 2025

### KICAD SKETCH

I finalized the connections based off of the ERC in kicad. I then put x indicators on all the pins that I would not be connecting to in order to avoid confusion. I realized that some of the digital ground pins were labelled with analog ground. So, I replaced all of the wrong ones. 

![NeoPod_5/26/25](https://github.com/lsyzg/NeoPod/blob/main/Images/NeoPod_5-26-25.pdf)

### KICAD PCB LAYOUT

Today I figured out the dimensions of the pcb. I am aiming for 100mm by 120mm so that the pcb can comfortably accomadate all the IC's I plan on putting on. I started the layout process by grouping the IC's together with all their components. Then, I placed the IC's that would stick out of the board such as the screen, USB-Cs, audio jack, and keyswitches so that I would know what space I had to work with in terms of trace and IC placement. I also started wiring and finished both USB-C plugs, some resistors and capacitors on the ESP32-WROOM32, and the DAC and audio jack. I did not finish the battery circuit, the SPI to UART converter, the RP2040 and microSD card reader, or the multiplexer.

![NeoPod_pcb](https://github.com/lsyzg/NeoPod/blob/main/Images/NeoPod-pcbnew_5-26-25.png)

TIME SPENT: 2hrs
