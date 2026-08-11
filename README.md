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
