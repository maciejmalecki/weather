# WiFi Weather Station with ESP8266
Here I describe a Weather station that I have assembled in few pieces and located in different places of my house. The station is able to collect several readings (temperature, humidity and optionally an air pressure) periodically and publish it via radio network.

The Weather station has following features:
* radio communication via WiFi,
* support for DHT sensor,
* deep sleep mode with additional "active" mode selector via momentary switch,
* suitable for battery powering,
* measures voltage level of power source (battery),
* can be powered with single 4.2V Li-ion cell due to integrated LDO voltage regulator and voltage stabilizer,
* usage of MQTT as integration backbone (in this case it integrates with Home Assistant-based central hub).

The Weather station design is based on ESP8266 module, namely the ESP12F variant. Alternatively, prototyping boards like Wemos D1 Mini can be used, at price of higher memory consumption thus shorter battery life.

Let's take a look at the overall architecture of the system, where Weather station coexists.

![WiFi topology](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.github.com/maciejmalecki/blog/master/sh/diagrams/wifi-topology.puml)

## Schematics

![Schematics](img/weather_schem.png)

### ESP8266 MCU
The weather station is built around ESP-12F, which is an ESP8266 variant.

The device uses following pins of ESP-12F MCU:
* VCC, GND - for powering it up with 3.3V,
* GPIO0 - for wake up circuitry,
* GPIO4 - input from weather sensor,
* GPIO14 - input for mode selection switch,
* ADC - analog input for voltage measurement circuitry,
* RESET - for manual reset and wake up circuitry,
* CH_PD - must be kept high.

### Power supply unit
The device uses single Li-ion 4.2V cell to power itself. The voltage is lowered to nominal 3.3V with LDO voltage regulator (U1, Microship MCP1700-3302E/TO). This regular has a very low dropout voltage (178mV) and can deliver up to 250mA, which is sufficient for ESP8266 and DHT11 sensor. Two capacitors C1 and C2 are used to further stabilize the output voltage. This should ensure stable operation of ESP microcontroller during wake-up procedure.

### Voltage measurement circuitry
The device measures Li-ion cell voltage via ADC analog input. Voltage readouts are used for both reporing and to implement cell discharge protection (the voltage of Li-ion cell should never drop below 3V). Because ESP 8266 module (here ESP-12F) accepts only 0-1V on ADC pin, I use voltage divider to reduce voltage before it reaches ADC input. Two resistors R3 and R4 with relatively high resistance (220K and 68K respectively) are used to ensure low current flow thus reduce power consumption.

### Mode selection, deep sleep and reset circuitry
By default, the device connects to the WiFi network and MQTT server, gather sensor readings and publish them and then goes for deep sleep for predefined amount of minutes. 

Reset switch (S1) shorts reset pin and GND. Reset pin must be pulled up (R2).

To ensure HTTP access to the firmware (ESP Easy) there is a special mode of operation that can be chosen during power-up or reset. When S2 switch is pressed for a few seconds after power-up or reset, the device enters config mode in which no deep sleep is used. Config mode can be turned off by subsequent reset (mode selection switch must be in released state).

To ensure that ESP chip can wake up itself, RESET and GPIO0 pins must be connected.

### Sensor circuitry

## Firmware

## Hardware assembly

### Bill of materials
| #   | Code | Name        | Description                | Price (PLN) |
|----:|:----:|-------------|----------------------------|------------:|
|1    |ESP12F| MCU         | ESP-12F                    |12.99        |
|2    |      | ESP Board   | SMD to THT adapter board   |2.90         |
|3    |      | PCB         | Universal board 4x6cm      |2.99         |
|4    |DHT11 | DHT11       | Temperature and humidity sensor|4.80     |
|5    |U1    | LDO Regulator|Microship MCP1700-3302E/TO |1.59         |
|6    |R1    | Resistor    | 10K                        |0.02         |
|7    |R2    | Resistor    | 10K                        |0.02         |
|8    |R3    | Resistor    | 220K                       |0.02         |
|9    |R4    | Resistor    | 68K                        |0.02         |
|10   |C1    | Capacitor   | Electrolitic, 1000uF       |0.50         |
|11   |C2    | Capacitor   | Ceramic, 100nF             |1.39         |
|12   |S1    | Switch      | Tact switch                |0.23         |
|13   |S2    | Switch      | Tact switch                |0.23         |
|14   |      | Cell basket | 18650 cell basket          |2.19         |
|15   |      | Li-ion cell | 18650 4.2V 10800mA cell    |6.50         |
|16   |      | Case        | ABS case with ventilation  |10.00        |
|     |      |             |                   **Total**|**46.39**    |

### PCB design
![PCB](img/weather_pcb.png)
