# Light-sensor-STM32-PLC-HMI-Project
Automatic light-based LED control system using STM32, BH1750 sensor, RS485/Modbus protocol, PLC ladder logic and HMI interface

# Fritzing Circuit Description
**MCU:** STM32F103C8T6 (Blue Pill)
**Connections:**
1. **BH1750 Light Sensor (GY-30):**
   - VCC → 3.3V
   - GND → GND
   - SCL → PB6 (I2C1_SCL)
   - SDA → PB7 (I2C1_SDA)
   - ADD → GND (address 0x23)
2. **RS485 Module (TTL485):**
   - VCC → 5V
   - GND → GND
   - RXD → A9 (USART1_TX)
   - TXD → A10 (USART1_RX)
3. **LED:**
   - Anode → PB0 (via 220Ω resistor)
   - Cathode → GND
**Notes:**
- BH1750 uses 3.3V logic (⚠️ NOT 5V!)
- RS485 uses 5V logic
- Cross-connect TX/RX for RS485 module




 ## PLC Ladder Logic - Light Control System

### System Overview
This PLC program (written in Delta ISPSoft, ladder logic) controls 9 lamps based on ambient light conditions, using two independent classification systems. The system runs inside a Step (STL) structure with Start/Stop control.

### Step Structure
The program uses two states:
- **S0** — Initial/Idle state. Waits for the Start command.
- **S20** — Running state. All classification and control logic executes here. Returns to S0 when Stop is pressed.

### Start/Stop Control
- **X0** — Start button (momentary). Transitions system from S0 to S20.
- **X1** — Stop button (momentary). Transitions system from S20 back to S0.

### Lamp Count Classification
Based on how many of the 9 lamps are currently ON, the system classifies the required sensitivity level:
- 0–2 lamps ON → **Less Light**
- 3–6 lamps ON → **Average Light**
- 7–9 lamps ON → **More Light**

### Lux-Based Classification
A simulated light sensor value (0–2000 lux) is classified as:
- Lux < 10 → **Less Light**
- 10 ≤ Lux ≤ 1000 → **Average Light**
- Lux > 1000 → **More Light**

### User Setpoint Logic
The user confirms the current lux reading and sets a desired ambient light target via HMI (D1 = actual lux, D2 = desired setpoint). The PLC compares these values and determines whether to turn lamps ON or OFF to reach the target.

### Register Map
| Address | Type | Description |
|---------|------|-------------|
| X0      | Input | Start button |
| X1      | Input | Stop button |
| Y0–Y8   | Output | 9 lamps |
| S0      | Step | Idle state |
| S20     | Step | Running state |
| D0      | Register | Count of lamps currently ON |
| D1      | Register | Current lux value (simulated, later from sensor) |
| D2      | Register | User-defined target lux (from HMI) |
| M0–M2   | Internal | Lamp-count classification (Less/Average/More) |
| M10–M12 | Internal | Lux-based classification (Less/Average/More) |
| M20–M21 | Internal | Control flags (need to turn ON / need to turn OFF) |

### Ladder Logic (STL Structure)