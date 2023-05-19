[![Power µModule](https://img.shields.io/static/v1?label=&message=µPower%20Module&color=gray&logo=Github)](https://github.com/tw1chao/micro_PowerModule)
[![license - CC BY-SA 4.0](https://img.shields.io/static/v1?label=license&message=CC+BY-SA+4.0&color=fcaabe)](https://creativecommons.org/licenses/by-sa/4.0/)
[![forks - µPower Module](https://img.shields.io/github/forks/tw1chao/micro_PowerModule?style=social)](https://github.com/tw1chao/micro_PowerModule/fork)
[![stars - µPower Module](https://img.shields.io/github/stars/tw1chao/micro_PowerModule?style=social)](https://github.com/tw1chao/micro_PowerModule/stargazers)
[![Donate-kofi](https://img.shields.io/static/v1?label=&message=Donate&color=434B57&logo=ko-fi)](https://ko-fi.com/yingchao_tw)
[![Donate-Paypal](https://img.shields.io/static/v1?label=&message=Donate&color=009cde&logo=Paypal)](https://paypal.me/tw1chao)


# µPower Module

1. Purpose：

 - DC/DC Programmable small-wattage power supply
 - This project is [Open Source Hardware](https://www.oshwa.org/definition/chinese/)


2. Scope：
 - RFQ
     1. MCU auto-regulator function
     2. Real time display of voltage and current
     3. Support [MikroBus](https://www.mikroe.com/mikrobus) Foot Position
     4. Expansion of the MAX6675 Module K-Type Thermocouple Temperature Sensor using the SPI interface in the Mikrobus hardware interface Thermocouple Sensor
     5. PCB size and pinout support for [Arduino UNO](https://docs.arduino.cc/hardware/make-your-uno-kit)
     6. Display voltage/current/wattage [Nokia 5110 LCD](https://github.com/EleonoreMizo/pedalevite/blob/master/doc/datasheets/Philips%20PCD8544%20-%20IC%2C%2048x84%20pixels%20matrix%20LCD%20controller%20(Nokia%205110).pdf), [TI-Nokia 5110 LCD](https://www.ti.com/lit/ml/swrp182/swrp182.pdf), [Nokia5110 LCD ](https://components101.com/displays/nokia-5110-lcd)
     7. Use a led to display local or remote mode

# HW Block Diagram

<a href="https://app.diagrams.net/#Htw1chao%2Fmicro_PowerModule%2Fmain%2FDocument%2FHardware%20Block%20Diagram%2F%C2%B5PowerModule.drawio"> <img src="./Document/Hardware%20Block%20Diagram/uPowerModuleBlockDiagram.png"/> </a>
<BR/>
plug-in module. ex. Temperature sensor, etc.. <br/>

# SW FlowChart
```mermaid
flowchart TD
subgraph "System Initialization"
  a("Check Communication") --> b(Check ADC) --> c(Read Sensor) --> d[buzzer <br> Make a sound]
end

subgraph "main"
    A(Start) --> B[System<br>Initialization]
    B --> C[Select Mode]
    C -->D["Remote <PC>"]
    C -->E["Local <encoder>"]
end

```


# Cost

|item|Category|Manufacturer|Product Number|Price </BR> (Mouser / Digikey) |
|----|--------|------------|--------------|--------|
|1|Microcontrollers|NXP (maybe modify to stm32)|S9KEAZN8ACTG| [NT\$96.79](https://mou.sr/3LNAdIo) / [NT\$94.00](https://www.digikey.tw/short/0vqm55z1)|
|2|Voltage Regulators|Monolithic Power Systems (MPS)|MP2315|[NT\$87.56](https://mou.sr/3M9TaXc) / [NT$98.00](https://www.digikey.tw/short/2qjpbj48)|
|3|Digital Potentiometers|Microchip Technology|MCP4019T|[NT\$24.97](https://mou.sr/3HOJMpd) / [NT\$25.00](https://www.digikey.tw/short/0jqd2wv8)|
|4|Transceivers|Texas Instruments|SN65HVD75DR|[NT\$93.03](https://mou.sr/3HQVWxX) / [NT\$97.00](https://www.digikey.tw/short/pb8hhdhp) |

---

## NOTE

For using STM32F030F4, 3.3 volts is required to provide MCU operating voltage; for Renesas or Infineon series, 5 volts is available, but **unit price is higher** :( 

### µModule Board Protocol

LIN bus MCU is easy and cheap to operate, but not easy to control by computer.
RS-485 price is higher, and the computer can have a module that can control directly.

---

# Candidate Components

The required hardware units are divided into the following 6 items.
1. Power
2. Microcontroller
3. Interface
4. Sensor (Current/temperature)
5. Connector
6. Debug port

## Voltage Regulators

### LDO
1. 3.3V [L78L33ACUTR](https://mou.sr/3NOMRcP)  NT$21.21 
2. 5V [MIC5219-5.0YM5-TR](https://mou.sr/3HTMLwK) Output Current: 	500 mA NT$43.10
3. 5V [R1172N501D-TR-FE](https://mou.sr/3HPtoog) Output Current: 1A  NT$41.73 	

### Buck - Switching Voltage Regulators
1. [MP2315SGJ-P ](https://mou.sr/3M9TaXc)
    > 1. [Mini low voltage module qs-1205cme-3a](https://dientu360.com/module-ha-ap-mini-qs-1205cme-3a)
    > 2. [【造物人】《可統編》1205CME-3A mini 超小體積 DC-DC可調降壓模組 效率97.5% 3A輸出 車載](https://shp.ee/5mihzr4)
    >   
    > **QS-1205CME-3A SPECIFICATIONS**
    > - Using IC: MP2315GJ-Z TSOT23-8
    > - Input voltage: DC 4.5-24V
    > - Adjustable range (0.8-17V), fixed voltage (1.8V 2.5V 3.3V 5V 9V 12V)
    > - Efficiency: 97.5% at voltage 5 - 6.5V / 0.7A, 94% at voltage 5 - 12V / 1A.
    > - Switching frequency: 1MHz (normal), 1.5Mhz (max)
    > - Output current: 3A (peak value), 2.1A (long time)
    > - Output ripple: < 30mV
    > - Dimensions: 20x11x5mm
    > - Weight: 2g 

### Boost
1. [MT3608](https://www.olimex.com/Products/Breadboarding/BB-PWR-3608/resources/MT3608.pdf)

## ARM MCU 

### Cortex-M0
1. [STM32F030F4](https://mou.sr/3nFtGYc)  NT$59.86 <!-- (https://www.st.com/en/microcontrollers-microprocessors/stm32f030f4.html) -->
    > STM32 does not support 5V operating voltage
2. [Infineon Technologies XMC1202T016X0016ABXUMA1](https://mou.sr/3B9SrP6)  NT$81.40
3. [Renesas Electronics R7FA2E1A52DFJ#AA0](https://mou.sr/3nCrYa1)  NT$85.85 

### Cortex-M0+
1. [NXP S9KEAZN8ACTG](https://mou.sr/3LNAdIo) NT$96.79


## [Provisional] Digital Potentiometer ICs 
> Perhaps DAC ICs can be used instead of Digital Potentiometer ICs 
1. [MCP4019T](https://mou.sr/3HOJMpd)  NT$24.97 
2. [MCP4725](https://mou.sr/3IfzNtm) NT$46.17

## Interface

### LIN Bus Transciver
[MCP2003BT](https://mou.sr/41gydxO) NT$32.83 

### RS-485
1. [SN65HVD75DR](https://mou.sr/3HQVWxX)  NT\$93.03 
2. [MAX485](https://mou.sr/42KdF2u) NT\$189.48
    > These modules can be purchased and disassembled at a lower cost.
    > <a href="https://shp.ee/ipzyxwm"> <img src="https://ae01.alicdn.com/kf/HTB1D1GINsfpK1RjSZFOq6y6nFXas/5PCS-LOT-MAX485-module-RS485-module-TTL-turn-RS-485-module-MCU-development-accessories.jpg" width="250"/> </a>

## Sensor

### Current Sensor
1. [ACS712](https://www.velleman.eu/downloads/29/infosheets/acs712_datasheet.pdf)
2. [INA219](https://mou.sr/45781sN)
    > These modules can be purchased and disassembled at a lower cost.
    > <a href="https://shp.ee/ipzyxwm"> <img src="https://www.vdrelectronics.com/storage/img/uploads/29814634.jpg" width="250"/> </a>

# Reference

## Protocols Reference 
1. [Chroma](https://www.chromaate.com/en/index) <img src="https://www.atecorp.com/ATECorp/media/images/Manufacturers/__thumbnails/chroma_logo_shadow_jpg_90_31.jpg" width="80">
   
2. [Kikusui](https://global.kikusui.co.jp/products-index/dc/) <img src="https://kikusui.co.jp/kiku/wp-content/themes/kiku2021/img/kikukoshiki-logo-n.svg" width="100">

3. [GWinsTek](https://www.gwinstek.com/en-global) <a href="https://www.gwinstek.com/en-global/products/downloadSeriesDownNew/6636/1330"> <img src="https://www.gwinstek.com/template/images/gwinstek_logo.png" width="100"/> </a>


# License & Disclaimer

<a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a> <br/> This work is licensed under a <a rel="license" href="https://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

If you have other ideas or suggestions, please feel free to submit an issue.
Welcome fork this repositories to provide contributions, or [buy me a coffee](https://ko-fi.com/yingchao_tw). thanks :)