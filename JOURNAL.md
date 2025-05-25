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
- Now onto the DAC, which I will use a PCM5102 breakout board from Adafruit for. 
- The microSD slot will just be standard chassis. 
- For the 5 buttons, I might go for mechanical keys and keycaps with up, down, right, left arrows
- The USB-C connector will be a 16 + 8 receptacle (USB4115-GF-A) that can transfer data and charge, which I will use a STC4054GR for.
- If I want a method to debug over USB-C from my computer, I can use a CP2102 that is between the D+ and D- lines to convert USB to UART and send data over serial and charge the device. 
- For file transfer to the microSD, I will use a separate USB-C receptacle, a RP2040 with a 12MHz XTAL with 2 22pF capacitors, and a 4x multiplexer(SN74CB3Q3257DBQR) that switches access of the microSD between the rp2040 and the ESP32-WROOM when the cable is plugged in via sel pins
- I will use a 1.8" to 2.0" TFT LCD (ST7789) that I can use SPI with
- After some research, a 3000 mAh 3.7v li-on battery should be sufficient to run the NeoPod for a while
- Add a momentary switch on the side to act as a [power button](https://randomnerdtutorials.com/esp32-deep-sleep-arduino-ide-wake-up-sources/), it will be connected to gpio0 of the ESP-WROOM so that it goes into and out of deep-sleep on press, use EXT0 on GPIO0

TIME SPENT: 4hrs
