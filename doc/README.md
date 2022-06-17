# EEERover Technical Guide

 During the Spring Term you should put your high-level design ideas into practice by creating the circuits, software and hardware that will implement your EEERover. This guide has three main aims:
 - Document the additional hardware that you are provided with to get started
 - Specify the processes you will need to develop your design (e.g.~ordering components)
 - Provide some implementation ideas that you may find useful
	
 Most of the contents of this guide is advisory. You do not have to use your EEEBug or the items in the starter kit if you wish to pursue different ideas.
	
 ## Expansion Kit
 You are provided with some parts to use with your Lab-in-a-Box kit to get started:
 
| Qty         | Description |
| ----------- | ----------- |
| 1†‡         | Exorock Moon Rock Simulator  |
| 2           | H-Bridge Motor Driver Module |	
| 2           | Adafruit Metro M0 Microcontroller Module |
| 2           | Adafruit WINC1500 WiFi Shield |
| 2†          | Magnet |

† *To follow when available*

‡ *A second Exorock may be available later, subject to availabilty*
			
 You are given one expansion kit per group in your allocated shelf. Use it with your existing EEEBug chassis. At the rear of the chassis is a mounting point for the Metro board and the central PCB has mounting points for the motor driver module. The breadboard, as you have already seen, is ideal for prototyping circuits but a number of mounting holes have also been provided for you to attach additional circuitry and components if you need more space. Finally, the design of the chassis is available to download that you can produce your own version.
    
### Motor Driver

The project kit contains a motor driver module to make it easy to control power to the motors.

<img src="https://user-images.githubusercontent.com/4660308/168389450-4aef4f97-12a5-4336-b253-555195959f80.svg" alt="Circuit diagram of an H-bridge with logic decoder" width="400"/>

The motor driver is a two-channel integrated *H Bridge* with decode logic, illustrated above. One channel controls the left motor and one channel controls the right motor. Each channel has two digital inputs, DIR and PWM. The digital inputs are decoded and used to control four MOSFETs, which are low-impedance switching transistors. The inputs can be connected directly to digital outputs on the Metro board. DIR controls the motor direction and the motor speed can be controlled by applying a rectangular waveform to the PWM input and varying the duty cycle - use `analogWrite()` in the Arduino API to generate a PWM signal. The motor driver module plugs into the motor power connectors on the EEEBug PCB.
			
### Metro M0 board

The Adafruit Metro M0 Express board is a embedded computing platform based on an ARM M0 microcontroller and compatible with the Arduino form factor, libraries and development tools. It is programmed in C++ and features digital inputs and outputs, inputs with digital-to-analogue conversion and PWM outputs. Internet connectivity is provided by a Adafruit WINC1500 WiFi Shield, which plugs into the Metro board.

You are provided with demonstration code that shows the use of basic functions of the Metro board. The WiFi interface is used to host a basic webpage, which you will see hard-coded into the demonstration code. The webpage provides two links which, when pressed, turn on and off the onboard LED. Use the Arduino Serial Monitor to connect to the board via USB and you will see some debug messages reporting on the connection status. Whenever the webpage is retrieved you will see the HTML request made by the web browser.
			
Arduino is a popular platform for prototyping and you will find a lot of information and many code examples on the internet. Use these resources but beware that many are provided by third parties and may not be well-written or even correct!
			
### Starter code

This repository contains [microcontroller code](../metro-starter-arduino/README.md) to get you started. You can continue to use Arduino IDE as in the lab skills work, or you can develop with Visual Studio Code and Platformio.
			
 ### Connections and Power
			
The logic level of the Metro board is 3.3V, which is supplied by an on-board voltage regulator. **You will damage the board if you connect the input/output pins to a voltage greater than 3.3V**. Use potential dividers to reduce voltages greater than 3.3V and measure the output voltage of your circuits before connecting them to the inouts of the Metro board. The safest option is to design add-on circuits with a 3.3V power supply. Digital inputs should be 0V for logic low and 3.3V for logic high. Analogue inputs should be in the range 0--3.3V.

![Metro M0 Express Pinout](https://cdn-learn.adafruit.com/assets/assets/000/110/930/original/circuitpython_Adafruit_Metro_M0_Express_Pinout.png)
*Pinout for the Metro M0 Express module from [Adafruit](https://learn.adafruit.com/adafruit-metro-m0-express/pinouts)*

The pin locations and functions are broadly compatible with the Arduino layout used on the Orangepip Kona328 board used in the Microcontroller lab experiment. The I/O pins pass through the WiFi Shield when it is connected, but the pins labelled CS, IRQ and RST on the WiFi Shield (Arduino pins 5,7 and 10) are used by WiFi and can't be used for other purposes. 

The Metro board can be powered by USB, the power jack or the EEEBug batteries. To connect to the EEEBug batteries you will need to make the following connections between the Metro board and the EEEBug PCB:

- 5V → 5V
- GND → GND

**Always check these connections before switching on the power. You may damage the microcontroller if you make a mistake**
			
You can use the 5V or 3.3V nodes to power your own circuit. It is not recommended to use Vin or Vbatt, since the voltage of these will vary depending on the power source. The motors will run slowly if the battery is not available.
			
<img src="EEERover-power.svg" alt="Simplified circuit diagram of the Metro M0 Express and EEERover PCB power supplies" width="600"/>

*A simplified diagram showing the connection of the Metro M0 Express and EEERover PCB power supplies*
			
 ### Exorock mineral simulator
      
 Your prototype will be developed and tested using Exorock simulators: electronic devices which can replicate the properties of the minerals that you are designign your rover to search for. An Exorock is  included in your kit to aid development.
 
<img src="https://user-images.githubusercontent.com/4660308/168095519-44b9d1c9-ec1a-468f-af53-7b428f99cf31.jpg" alt="Photograph of PCB inside Exorock" width="600"/>
					
The Exorock is configured by setting a group of DIP switches on the PCB. A push button is used to turn it on and off, and an LED indicates when it is active. A flow chart depicting the operation of the Exorock is shown below. From standby (inactive) mode, press the button to turn on and observe the LED flashes to determine whether the battery has sufficient charge. After the battery display sequence is complete the Exorock begins emitting signals according to the mode set up on the DIP switches. The LED flashes a code at intervals to confirm the mode selection and indicate operation.

<img src="exorock-flow.svg" alt="Simplified circuit diagram of the Metro M0 Express and EEERover PCB power supplies" width="600"/>
		
Pushing the button during operation will reset the Exorock --- this is necessary to change the operation mode. A second press soon afterwards will turn the device off. If the Exorock is left operating for five minutes it will automatically power off to preserve the battery.
			
The Exorock cannot simulate the magnetic property in an readily-configurable manner so you are provided with a small magnet to test this functionality. In the demo, magnets will be placed inside the Exorock approximately 10mm below the highest point on its top. The polarity of the magent in your kit depends on the colour of the disc:

<img src="https://user-images.githubusercontent.com/4660308/170473263-a0c64335-e151-4071-b97a-f077de7aaca5.jpg" alt="With the disc flat on a surface, the magnetic field is orientated up from a blue disc and down with an orange disc" width="600"/>
			
The emitted infrared and ultrasonic signals are strongest above the exorock. The radio signal is emmitted from a coil antenna in the plane of the PCB.	
			
 ## Components and Construction
 ### Ordering components

A tool for ordering components will be released later in the term. Each project group has a budget of £60 to cover all the additional components you'll need for designing and building your prototype.

 - [ ] Add ordering tool

You will receive an email when the parts are available for collection from Stores on level 1, near the lifts. Orders, assuming the items are in stock, will take up to a week to arrive. You can order some components directly from stores stock and these will usually be available for collection sooner.
			
Please be aware of the following rules and common pitfalls when ordering components:
 - You must only order items that you can use safely. If you have any doubts about this then talk to a member of staff before ordering. Do not order chemicals (including glue), tools or mains-powered equipment/parts without consulting staff.
 - Always check that the supplier holds sufficient stock. If an item is out of stock it will be placed on back-order and it may take months to arrive. Try to find an alternative part or supplier instead.
 - Some suppliers offer stock from sister companies around the world. Do not order any items which warn about additional delivery charges.
 - Many electronic components are *surface mount*. This means that they sit on flat pads on a PCB instead of having flexible leads to insert into a prototyping board. They are also usually very small. You will find surface mount components difficult to use so avoid them by checking item descriptions and datasheets carefully.
 - You need to maintain the goodwill of the stores staff to order parts for this project and future projects. Do not chase the staff for your order until one week has passed from placing the order. It is your responsibility to check parts for suitability and availability.
			
 ### Making a new chassis
 This repository contains design files for the EEEBug chassis. You can design your own version using this as a template. You will see that cut lines are drawn in red and engrave lines are drawn in blue. Laser cutting removes a tapering width of material known as a *kerf*, which for 3mm acrylic is typically 0.1mm at the bottom of the material and 0.3--0.4mm at the top. Hence, cut edges are not vertical, external dimensions will be slightly smaller than drawn and internal dimensions slightly larger than drawn.

Designs for laser cutting must be submitted in PDF format. Multiple parts can be placed on the drawing as long as they fit into an area of 600mm x 300mm. Add engraving to identify your group as the owner. You are strongly recommended to check your design by printing it on paper first - the scaling will be identical as long as the PDF file is printed at 100% scaling.

 - [ ] Confirm chassis ordering process
			
### 3D printing and Workshop facilities
A basic workshop is available in the lab. To use it talk to Vic and be prepared to explain exactly what you want to do and demonstrate that you are capable of doing it safely. You can also have components 3D printed if you are willing to learn the necessary design techniques independently. Talk to Amine in the technicians' office if you would like to use 3D printing.
			
## Implementation Hints
The table of mineral types from the design brief is:

| Mineral | Property 1 | Property 2 |
| ------- | ---------- | ---------- |
| Gaborium | 61kHz radio modulated at 151Hz | Acoustic signal at 40.0kHz |
| Lathwaite | 61kHz radio modulated at 239Hz | None |
| Adamantine | 89kHz radio modulated at 151Hz | Magnetic field up |
| Xirang | 89kHz radio modulated at 239Hz | Magnetic field down |
| Thiotimoline | Infrared pulses at 353Hz | None |
| Netherite | Infrared pulses at 571Hz | Acoustic signal at 40.0kHz |
			
  ### Radio
Four material types emit radio waves, but you can choose the amount of discrimination that you can provide. Do you need to distinguish between the carrier frequencies? Do you need to measure the modulation frequency?
			
You can use a tuned coil antenna to pick up the signal. This is essentially an air-cored inductor with a reasonably large diameter, which you can make by coiling wire. This inductor can then be used in a tuned circuit that will be most sensitive at the resonant frequency. The lab has *LCR Bridge* instruments that can tell you the inductance of your inductor. There are only two carrier frequencies to detect so you do not need the continuous adjustment of tuning that you get with an audio radio receiver, for example.
			
The signal amplitude generated by your antenna will be small so it will need amplification before you can use it to indicate detection. The microcontroller will struggle to sample the radio signal at its carrier frequency so you will need to perform demodulation with *envelope detection* to produce a signal which indicates the amplitude of the signal. The easiest way to do this is by rectification. The on-off modulation is a form of *amplitude modulation*, where the relative amplitude of the signal varies between 1 and 0.
			
 ### Infrared
You have already made a light sensor as part of the EEEBug and most silicon-based photosensors (such as the EEEBug phototransistor) are just as sensitive to infrared as visible light. However, the optical power given by the Exolizard is weaker than the light source you used for the EEEBug so you may need to amplify the output. You may need to measure the pulse frequency of the signal and this can be done with analogue or digital methods.
			
 ### Ultrasonic and magnetic
The magnetic field of the Exolizard is static, meaning that it will not induce a current in a coil of wire unless that coil is moving. A moving coil is a possibility, but there are also sensors and switches that can detect a static magnetic field. Test any new components and read their datasheets to determine if any amplification or signal processing is required. The polarity of the magnetic field is not specified, so make sure you can detect either polarity.
			
Sensors are also readily available for detecting ultrasonic signals. Amplification may or may not be necessary, depending on the sensitivity of your sensor. As with the radio receiver, rectification will convert the high-frequency audio signal into a voltage representing the amplitude. Most ultrasonic transducers have a resonant frequency at which they are most sensitive. Check component datasheets to confirm the frequency sensitivity of any sensor you are planning to use.
			
 ### Opamps
You may wish to implement amplification or filtering using an opamp. There are many different opamps and they have different specifications. Some manufacturers have parametric search facilities on their websites to narrow down the options based on your specifications. Look at the appendix of the opamps experiment instruction to see an example of how the LT1366 device was selected for that application. You can use the LT1366 breadboard modules from your Lab-in-a-Box kit in your rover, but do not modify them or solder them.
