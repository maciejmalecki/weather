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
The weather station uses following pins of ESP8266 MCU:
* VCC, GND - for powering it up with 3.3V,
* GPIO0 - for wake up circuitry,
* GPIO4 - input from weather sensor,
* GPIO14 - input for mode selection switch,
* ADC - analog input for voltage measurement circuitry,
* RESET - for manual reset and wake up circuitry,
* CH_PD - must be kept high.

Theoretically each ESP module with following pins exposed is suitable. With some concession it should be also doable to implement this device with ESP-01 module.

### Power supply unit

### Voltage measurement circuitry

### Mode selection, deep sleep and reset circuitry

### Sensor circuitry

## Firmware

## Hardware assembly

### Bill of materials

### PCB design
![PCB](img/weather_pcb.png)
