# JK BMS

There are many projects reverse Engineering protocols to interact with a jk-bms in a smart home enviroment.
In contrast, information about the actual hardware are sparse eventhough the active balancing circuit is
quite interesting. This repository attemps to give an overview about the Hardware used inside the BMS.

##Microcontrollers

The Brain of the BMS is a CH32F103C8T6 which is a clone of the popular STM32F103 microcontroller. There are 
no obious probe points visible to access the SWD debugging interface or a UART shell. The CH32 is clocked by a 8Mhz quartz

Bluetooth connectivity is provided by a BK3432 Bluetooth SoC in form of a soldered module. There are just a
few connections to the PCB so it can be assumed that the module only handles communication and no other control tasks.

##Input Stage
Each cell input has a small RC filter with a 10mOhm. This seems to be more like an inrush current limitation and I cant realy see
a reason for this. The capacitors are configured as differential filter capacitors instead of ground referenced ones
	
Cell N _____----____________
            ----    __|__
	                _____
Cell N-1______________|_____

	        
After filtering, the cell inputs are connected to bidirectional MOSFET switches with positive poles of even numbered cells mounted on the top and odd numbered cells on the bottom of the PCB.
Each switch is controled via a chain of four shift registeres near the CH32 microcontroller. Level shifting is achieved by NMOS and PNP based level-shifters. The following Image shows the
reverse-engineered schematic of the input stage for a single cell

![]{images/Input_Stage.PNG}

The output of all stages on the same side of the PCB are connected together and are feed into a second stage of bidirectional switches. The resulting switching scheme is shown in the following figure:

![]{images/Switching_Scheme.PNG}

Via this scheme it is possible to select a single cell that is feed into the balancing circuitry as well as to an ADC for cell voltage measurements.

##Balancing circuitry

##Current Sensing and  