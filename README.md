# EG4-6000XP-Home-Assistant-Local-Control
"Ditch the cloud. Fast, local monitoring and control  for the EG4 6000XP via RS485 and Home Assistant."
This project provides the register maps, wiring guides, and configuration files to manage your inverter locally.🔌 Hardware SetupTo bypass the WiFi dongle, you must use a wired connection to the INV485/CAN port.1. Wiring the RJ45 CableUse a standard CAT5/6 cable and strip one end. Connect only two wires to your RS485 adapter:Pin 8 (Brown): Connect to A+ on your adapter.Pin 7 (Brown/White): Connect to B- on your adapter.Note: Ignore Ground (GND); it is typically not required for this isolated link.2. Waveshare Adapter SettingsIf using the Waveshare USB-to-RS485 adapter:Set DIP Switches 1 & 2 to UP (Engages 120Ω termination).Baud Rate: 19200Slave ID: 1 (Default)💻 Home Assistant SetupInverter Sensor Map (Input Registers)Add this to your configuration.yaml. We use input_type: input (Function Code 04) for real-time data.YAMLmodbus:
```  - name: eg4_6000xp
    type: serial
    port: /dev/serial/by-id/usb-FTDI_Dual_RS232-HS-if00-port0
    baudrate: 19200
    sensors:
      - name: "EG4 Battery Voltage"
        address: 4
        scale: 0.1
        unit_of_measurement: "V"
        device_class: voltage
      - name: "EG4 Battery Current"
        address: 5
        scale: 0.1
        # If your idle current shows ~256A, add: offset: -256
        unit_of_measurement: "A"
        device_class: current
      - name: "EG4 EPS Power Total"
        address: 552
        unit_of_measurement: "W"
        device_class: power
```
Writing Commands (Control)To control  or limit charging, use the modbus.write_register service in your automations.ParameterRegisterRangeMax Charge Current1300 - 125AAC Input Max Power130(Varies by Firmware)🛠 TroubleshootingStatus 0.30 Hz: Normal reading when the inverter is in standby or has no AC load.No Communication: Double-check the DIP switches on the Waveshare and ensure you are using the correct serial port ID.Why this GitHub project is usefulMost guides only show how to use the WiFi dongle over the network. This repository is for users who want zero-latency, internet-independent control—critical for off-grid battery protection.Solar Assistant Beta Adds RS485 Support for Luxpower Inverters – Here's How on the EG4 6000XP!
This video is a great companion to your project as it shows the physical teardown and discovery of the RS485 pins on the 6000XP, which confirms the wiring logic used in your GitHub repo.
https://www.youtube.com/watch?v=T5a1D2DU5rI
