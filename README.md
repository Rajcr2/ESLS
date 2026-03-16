# Efficient Street Lighting System 

### Introduction

This was my academic project focused on creating a efficient LED based Street Lighting System which will avoid Power Wastage due to manual mismanagement.

### Motivation

I have observed many times that street lights were ON till 10 AM because operator forgot shut the main switch down and seen this very frequently & also lights were ON
1/2 hrs before SUNSET which leads to Power Wastage this is the Main reason or motivation behind this project.

<img width="435" height="500" alt="image" src="Images/Others/Motivation 1.png" />

<img width="416" height="500" alt="image" src="Images/Others/Motivation 2.png" />

### Objectives

1.	To make sure that all street lights will remain ON only during **DARK (Cloudy Condition)/NIGHT hours** & will remain off completely during **DAY HOURS (from Sunrise to Sunset)**.
2.	In case, If Power remains ON during Day-Time due to operators negligence LED lights will still remain OFF contributing to Energy saving.
3.	To Improve life of street light lamps.
4.	To Achieve Maximum Energy Efficiency. 

## System Design

<img width="1000" height="440" alt="System Design Clear" src="Images/Others/System Design Clear.png" />

### Components

| Component           | Purpose                             |
| -------------       | ----------------------------------- |
| **Arduino Nano**       | Microcontroller control unit        |
| **DS1307 - RTC Module** | Time scheduling                     |
| **MOC3021**             | Opto-isolated TRIAC driver          |
| **BT136 - TRIAC**       | AC load switching                   |
| **LDR Module**         | Detects ambient light intensity (luminance) |
| **470Ω Resistor**      | Limits current into optocoupler LED |

### Schematic Design

> The schematic is designed in KiCad. Schematic consists Arduino Nano (Control Unit), DS1307 RTC Module, optocoupler driver (MOC3021), BT136 TRIAC (switching circuit) for AC load along with Resistors.

<img width="772" height="537" alt="ESLS Schematic" src="Images/Others/ESLS Schematic.png.png" />

### PCB Layout

> The PCB layout separates the low-voltage control circuitry from the AC power section to maintain electrical isolation. Routing was optimized to keep the power path short and reduce noise coupling.

<img width="772" height="537" alt="PCB Layout - ESLS" src="Images/Others/PCB Layout - ESLS.png" />

### 3D View

<img width="1677" height="923" alt="ESLS_3D" src="Images/Others/ESLS_3D.png" />

### Design considerations

- **Isolation between low-voltage control circuit and AC mains section.**  
- **Clear routing of power paths for the TRIAC switching stage.**  
- **Through-hole components used for easier prototyping and soldering.**

## Circuit Simulation

### ngspice Simulation
**Transient analysis** was performed using ngspice integrated in KiCad to validate the Arduino driver stage.

> ### Limitation
> **Since microcontrollers cannot be simulated directly in SPICE, the Arduino PWM output was emulated using a pulse voltage source.**

- ### Simulated Circuit
<img width="420" height="318" alt="ngspice_schematic" src="Images/Simulation/ngspice_schematic.png" />

- ### PWM Signal Emulation

All required Arduino PWM signals were emulated using the SPICE source :

Example : **PULSE(0 5 0 1u 1u 1m 2m)**
| Parameter    | Value |
| ------------ | ----- |
| Low Voltage  | 0 V   |
| High Voltage | 5 V   |
| Rise Time    | 1 µs |
| Fall Time    | 1 µs |
| ON Time      | 1 ms  |
| Period       | 2 ms  |

This results in a 50% duty cycle PWM signal, which corresponds to the Arduino command:

`analogWrite(pin,128)`

- ### Transient Analysis Results

The transient simulation produced the following waveform.

<img width="1580" height="605" alt="ngspice_simulation" src="Images/Simulation/ngspice - Simulation.png" />

**Observations from the simulation** :
- The voltage waveform alternates between 0 V and 5 V, representing the PWM output.
- The current through the resistor and LED appears as periodic pulses synchronized with the PWM signal.

> **Calculation** :
>
> **I = (Vcc − Vf) / R**
>
> **Vcc** = 5 V, **Vf** ≈ 1.2 V (LED forward voltage), **R** = 470 Ω
>
> I ≈ (5 − 1.2) / 470
> 
> **I ≈ 8 mA**

- **The simulated current (~9.1 mA) aligns with this calculation and confirms that the Arduino output pin can safely drive the optocoupler input stage.**

### Proteus Simulation

To verify the embedded firmware behavior, the system was simulated in **Proteus**.

Unlike SPICE tools, Proteus allows simulation of **Arduino code execution along with peripheral modules such as the RTC**.

<img width="772" height="537" alt="ngspice_simulation" src="Images/Simulation/Proteus - Schematic.png" />

Simulation demonstrates and displays output Oscilloscope how the Arduino program changes the **output signal according to the RTC time conditions**.

### Simulation Outputs

- #### At Mid-Day Time
<img width="1677" height="923" alt="Proteus_Day_Hours" src="Images/Simulation/Proteus - Day hours.png" />

- #### At Evening Time
<img width="1677" height="923" alt="Proteus_Day_Hours" src="Images/Simulation/Proteus - Evening hours.png" />

- #### At Night
<img width="1677" height="923" alt="Proteus_Day_Hours" src="Images/Simulation/Proteus - Night hours.png" />

- #### At Early Morning
<img width="1677" height="923" alt="Proteus_Day_Hours" src="Images/Simulation/Proteus - Early Morning.png" />

- #### At Morning
<img width="1677" height="923" alt="Proteus_Day_Hours" src="Images/Simulation/Proteus - Morning.png" />

These results confirm that the firmware correctly generates the expected control signal based on RTC timing conditions. Using the same logic then implemented and validated on the physical hardware prototype.
__________
## Hardware Implementation

The circuit is implemented on a custom PCB designed in KiCad. 
The circuit board includes the Arduino Nano module, RTC module, LDR sensor module, MOC3021 optocoupler, 470Ω resistor, connectors, and a BT136 TRIAC.

The Arduino Nano is mounted on the PCB and programmed to execute the control logic based on RTC timing conditions. After assembling the PCB, the logic was tested on a working prototype to validate the system behaviour of the control stage.

### PCB
<img width="450" height="500" alt="image" src="Images/Others/ESLS_7.jpg" />

### Prototype
<img width="772" height="537" alt="image" src="Images/Others/1773557903896.jpg" />

## Results

**The hardware prototype was tested at different time conditions defined in the firmware.
The observed outputs confirm that the system correctly generates the control signal and operates as intended.**


- #### 1. During Day Time
<img width="1677" height="923" alt="Day Hours" src="Images/Others/Day Hours.png" />

- #### 2. At Evening
<img width="1677" height="923" alt="Evening Hours" src="Images/Others/Evening Hours.png" />

- #### 3. At Peak Night Hours
<img width="1677" height="923" alt="Night Hours" src="Images/Others/Night Hours.png" />

- #### 4. During Cloudy Conditions
<img width="1677" height="923" alt="Cloudy" src="Images/Others/Cloudy Day.png" />



