# UT23 Accumulator Control Module v.1.2

By: Benjamin Liang, Controller Design Lead, University of Toronto Formula Racing Team

In this article, I will present the Accumulator Control Module (ACM) v.1.2 for UT23, our car for the next competition season. Each year, we, the University of Toronto Formula Racing Team (UTFR) design and builds a new formula-style car from scratch to compete in international Formula SAE events. One of the main aspects of the competition is to ensure all elements of the vehicle comply with Formula SAE regulations to ensure the safety of students. The accumulator management system is amongst the most safety-critical systems on the car with the Accumulator Control Module (ACM) v1.2 being the center node. This PCB performs functions such as reading our battery temperatures, controlling our tractive system relays, state detection for our tractive system active light, interfaces with the isolation monitoring device and communicates with our main controller over CAN and other rules-mandated safety features.

![image](https://user-images.githubusercontent.com/82067858/205166643-3fefbf65-06b8-468f-8d58-750d702029db.png)
 
The ACM includes different components such as a Teensy 4.1, three CAN transceivers (two standard and one isolated), 5V and 3V3 linear regulators, 12V and 5V DC converters, MOSFET control and voltage detection circuits, unity gain isolation differential amplifiers, an SR latch, an optocoupler, and a comparator. A more detailed visual representation of the board architecture is shown in the figure below. In this article, I will go over how some of these components are used for CAN bus communication, relay control, and tractive system voltage measurement.

![image](https://user-images.githubusercontent.com/82067858/205166343-3c8b792d-b132-4145-8f92-4fb37aa93884.png)
 
Schematic Design

Specifically, the controller design revolves around a Teensy 4.1 as the MCU. The Teensy 4.1 is responsible for controlling the tractive system and shutdown circuit relays, monitoring battery and tractive system states, reading a series of safety-critical signals, and communicating with the Rear Controller, our car's main controller which operates our vehicle state machine.

![image](https://user-images.githubusercontent.com/82067858/205166319-a87adb48-ad19-4818-b26e-8d22788b4d85.png)
 
The Teensy 4.1 has three built-in CAN interfaces that it communicates with via UART that only require a CAN transceiver, adequate termination, and a bypass capacitor to become operational. The three CAN bus in the accumulator include sensor CAN (non-critical data acquisition bus), accumulator CAN (critical battery management bus) and thermal expansion module CAN (battery temperature measurement bus). The accumulator CAN bus is variably terminated with a reed relay (K3) in the schematic below to preserve termination when the battery is in running mode or in charging mode. A charger with a CAN bus termination resistor is connected to the accumulator CAN bus when in charging mode so the termination resistor on the ACM should be disconnected to run the bus within specification. The referenced circuits are shown in the schematic above.

![image](https://user-images.githubusercontent.com/82067858/205166281-f249f3a8-a587-4275-8a8f-1f71c05fde9a.png)
 
The accumulator isolation relays (AIRs) and pre-charge relay have auxiliary contacts for communicating the mechanical relay states. The AIRs and pre-charge relay are normally open and so is the pre-charge auxiliary contact, but the AIR auxiliary contacts are normally closed. To read all the auxiliary contact signals as active high closed signals, non-inverting MOSFET circuits must be used for the AIRs and inverting MOSFET circuits for the pre-charge relay. The figure above shows circuitry for controlling the relay coils and reading the auxiliary contact signals.

![image](https://user-images.githubusercontent.com/82067858/205166249-bd68046b-cfed-4419-837a-3d1ba2c93f3f.png)
 
The tractive system voltage measurement circuit uses a comparator with a reference voltage threshold to send a signal to the Teensy via an optocoupler when the tractive system is in a high voltage state (>60V). A 5V DC converter and optocoupler are used since the circuit is isolated from the low voltage system. Op amps are used to mitigate the effects of non-ideal input and output impedance on signal integrity. The tractive system voltage measurement circuit is shown above.

PCB Layout

![image](https://user-images.githubusercontent.com/82067858/205166203-917cfdc0-a45d-4810-a068-8be54bfca076.png)

The ACM is a four-layer board designed using Altium Designer as shown in the figure below. The four layers include a high-frequency digital signal and power top layer, a power and ground second layer, a grounded third layer, and an analog and low-frequency signal bottom layer. Furthermore, the ACM board layout includes three different isolated grounds, our low voltage system (includes all the control electronics), tractive system (HV system that provides power to the motor), and thermal expansion module (system responsible for temperature monitoring of our battery pack) ground. Traces were designed with their copper weight, layer thermals, and expected current in mind. Differential signals were routed near each other, and other high-frequency signals were far from one another while still preserving similar trace lengths. A substantial effort was placed in making a user-friendly silkscreen to ensure the correct and safe operation of the board as it will include high voltage and sensitive components.
 
JLCPCB

This PCB is the cornerstone of a rapid and thorough design cycle with the goal of de-risking our accumulator functionality so we can extend our testing season. This amazing technical and project management development within our team was only possible thanks to the help of JLCPCB (https://jlcpcb.com/RAT). Founded in 2006, JLCPCB (https://jlcpcb.com/RAT) is a leading global PCB manufacturer with over 16 years of experience providing the best customer experience through the rapid production of highly reliable and cost-effective PCBs.

![image](https://user-images.githubusercontent.com/82067858/205166072-44f0b8b3-9c9d-4a9f-8a78-1eb566052175.png)
 
The accumulator control module was ordered from JLCPCB (https://jlcpcb.com/RAT) for quick, reliable, and cost-effective manufacturing. Ordering PCBs from JLCPCB (https://jlcpcb.com/RAT) is extremely simple and consists of the following four quick steps:
1.	Export Gerber files from Altium Designer
2.	Click on the instant quote button on the JLCPCB website (https://jlcpcb.com/RAT)
3.	Add your Gerber files and double-check the uploaded PCB in the Gerber viewer
4.	JLCPCB automatically detects all the general PCB specifications and fills out the order form for you. Review and modify these to ensure they match your PCB requirements.
5.	Click save to cart, fill out your address, select your shipping method, and pay

Furthermore, JLCPCB (https://jlcpcb.com/RAT) also provides many other manufacturing services including an SMT service which fulfills the money and time saving needs of customers at only an $8.00 setup fee ($0.0017 per joint). It is a one stop online platform where you can also track the progress of your PCBA manufacturing process, which is completed within a day, in real time. Through the platform you can conveniently source thousands of components from JLCPCB (https://jlcpcb.com/RAT) and its reliable component partners like Digikey and Mouser. The quality and reliability are always superb with professionally trained engineers, X-ray inspection, automated optical inspection, automated component placement, and various soldering techniques. 
