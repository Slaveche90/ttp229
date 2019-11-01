# TTP229 Capacitive 4x4 Keypad Module  
This repository is for TTP229 Capacitive 4x4 Keypad Module connected to the Raspberry Pi 3  

TTP229 Capacitive 4x4 Keypad Module has two sets of jumpers which you have to connet to select the specific mode for the module. For this example script we selected a jumper JP2, which is for the 16 channel mode. All other jupers are disconnected.

# How to run the script  

To run the scrip, open the terminal app in the directory where you downloaded the script, then run this command:  
```
  pyhton3 ttp229.py
```

# Explanation of the code  

First we import libraries for the time and the GPIO pins. Then we set GPIO pin names to BCM, and disable warnings.

After this, we define and initialize two variables for pin names; SCL and SDO connected to the GPIO pins 27 and 10, respectively. Then we define and initialize two variables for detecting pad touch:
touch - variable for pad name, and
edge_detect - variable for detecting pad touch timing.

Then we set up modes for used GPIO pins; SCL as the output, and SDO as the input with the internal pull up resistor. Also, set the SCL line to the HIGH state.

After this we define the interrupt() function which we will explain later.

In the infinite loop block (while True:) we are waiting for the falling edge of the singal on the SDO pin, and only if there is the falling edge of the signal we call the interrupt() function. We do this because SDO line is always HIGH, and when we touch the specific pad, SDO line sends Data Valid pulse (active LOW). On the first image below marked with green point.

In the interrupt() function first we wait for additional 5 microseconds (after DV pulse). This is the time for the change of signal state, and you can find this on the two-wire signal diagrams in the datasheet (minimum is 4 microseconds). Then we change the clock signal. In the FOR loop, we generate 16 clock signal changes, and read SDO pin on every LOW state of the clock signal. When we touch the pad this is the time when the signal on the SDO pin change state from the HIGH to the LOW state. All we have to do, when we detect pad touch, is to save the loop cycle variable “i” to our TOUCH variable, then print the data. In every loop cycle, clock signal is on the LOW state for 10 microseconds, and also on the HIGH state for 10 microseconds. When we exit the for loop, the clock signal is on the HIGH state again. You can see these signal changes on the image below. 

![alt](https://github.com/Slaveche90/ttp229/blob/master/Signals.png?raw=true)

Also you can see that we touched the pad number 5 by counting clock signal changes before SDO signal changed the state to LOW (in the center of the image).  

If you experience any errors, make sure to choose the GPIO pins that have no other secondary functions (like I2C pins, or SPI pins). If you choose any GPIO pin with the secondary function, make sure to turn off the interface for that secondary function.

# Jumper names:  

![alt](https://github.com/Slaveche90/ttp229/blob/master/JumperNames.png?raw=true)

# Connection diagram  

![alt](https://github.com/Slaveche90/ttp229/blob/master/ConnectionDiagram.png?raw=true)  

**Keypad pin - Raspberry Pi pin**   
VCC - 3V3 - [pin 1] - Red wire  
GND - GND - [pin 6] - Black wire  
SCL - GPIO27 - [pin 13] - Green wire  
SDO - GPIO10 - [pin 19] - Blue wire  

**NOTE:** Only TP2 jumper is connected! (Orange wire)  