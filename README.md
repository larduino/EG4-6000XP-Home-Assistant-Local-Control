# EG4-6000XP-Home-Assistant-Local-Control

Ditch the cloud and take direct control of your EG4 6000XP via RS485 and Home Assistant. This project provides the register maps and configurations to manage your inverter locally.

## 🔌 Hardware Setup

### 1. Wiring the RJ45 Cable
Connect a standard CAT5/6 cable to the **INV485/CAN** port. Use the following pinout for your RS485 adapter:

| Function | RJ45 Pin (T-568B) | Waveshare Terminal |
| :--- | :--- | :--- |
| **RS485 A** | Pin 8 (Brown) | A+ |
| **RS485 B** | Pin 7 (White/Brown) | B- |

### 2. Adapter Configuration
* **DIP Switches:** Set 1 & 2 to **UP** (120Ω termination).
* **Baud Rate:** 19200
* **Slave ID:** 1

## 💻 Home Assistant Configuration

Add the following to your `configuration.yaml`. Note the use of `input_type: input` (Function Code 04) for real-time data.

```yaml
modbus:
  - name: eg4_6000xp
    type: serial
    port: /dev/serial/by-id/usb-FTDI_Dual_RS232-HS-if00-port0
    baudrate: 19200
    sensors:
      - name: "EG4 Battery Voltage"
        unique_id: "eg4_batt_v"
        slave: 1
        address: 4
        input_type: input
        scale: 0.1
        unit_of_measurement: "V"
        device_class: voltage

      - name: "EG4 Battery Current"
        unique_id: "eg4_batt_c"
        slave: 1
        address: 5
        input_type: input
        scale: 0.1
        unit_of_measurement: "A"
        device_class: current
```

⚠️ Disclaimer

Proceed at your own risk. Modbus allows for direct writing to inverter registers. 
Incorrect settings can damage your battery or inverter.
Always double-check register addresses before writing commands.
