# Wiring Guide

## RS232 (Single-Device)

Inverters equipped with a 9-pin D-SUB connector on the underside can be connected to a microcontroller using an RS232-to-UART-TTL converter module. In this configuration, however, multiple inverters cannot be daisy-chained. Each inverter must be connected individually to the microcontroller. If you want to read data from multiple inverters, it is recommended to use the RS485 bus as described below.

### Connector pinout

```text
9-pole D-SUB (male) pinout

---------------
\  1 2 3 4 5  /
 \  6 7 8 9  /
   ---------

Pin 2: Receive Data (RXD)
Pin 3: Transmit Data (TXD)
Pin 5: Ground (GND)
```

## RS485 (Bus)

Up to 99 Sunways NT inverters can be connected to a single RS485 bus. This allows all devices to be read by one master device without the need to connect each inverter individually to the microcontroller.
Each inverter must be assigned a unique address in the range of 1 to 99. Different types or series of Sunways NT inverters can be connected to the same bus, as long as each device has a unique address.

### Choosing a Cable

Sunways recommends using a shielded twisted-pair cable. In my setup, I used an old four-wire telephone cable that I had lying around. In general, any shielded two-wire cable should work, provided that the cable length is not excessive and that it is not routed close to strong electrical interference, which could negatively affect signal quality.

### Connection

**DISCLAIMER!!!**
The following steps require removal of the inverter’s cover. Inside, there are exposed high-voltage components that pose a serious risk of electric shock. Carefully consult the official manual before performing any of the steps described below. If you are not confident in carrying out this procedure, seek assistance from a qualified professional. No liability or warranty is assumed.

To connect the inverter to the RS485 bus, first disconnect it from the grid and shut it down completely (also on AC side). Wait at least five minutes before removing the cover to ensure that all internal capacitors are fully discharged. After the cover is removed, look for the connection terminals on the bottom of the inverter circuit board. There should be one labeled "RS485". Depending on the model of inverter the naming/position of the terminal might be different. Please look it up in the manual. Connect two wires of the cable to **"RS485+"** and **"RS485-"**. A third wire, or alternatively the cable shield, can be connected to **“RS485_GND”**.

### Connection diagram
```text
Inverter 1            Inverter 2
──────────            ───────────
RS485+    ─────────▶  RS485+
RS485-    ─────────▶  RS485-
RS485_GND ─────────▶  RS485_GND
```

### Termination jumper
Near the connection terminals, there should be a jumper labeled **“RS485MATCH”**. On the last inverter connected to the RS485 bus, this jumper must be set to **“RS485MATCH”** instead of **“SPARE”**. This is required because an RS485 bus must be terminated with a **120 Ω resistor** at the end of the bus to ensure reliable communication.

### Address configuration
To configure the inverter’s RS485 address, you can either use the setting in the display menu or remove the cover to access the menu buttons on the display board.
