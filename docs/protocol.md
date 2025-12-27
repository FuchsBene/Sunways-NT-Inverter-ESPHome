# Communication Protocol Description

## Serial Communication Parameters

- **Baud rate:** 9600 bps
- **Data bits:** 8
- **Parity:** None
- **Stop bits:** 1
- **Slave addressing (RS485):** 1-99


## Request Frame (5 Bytes)

| Byte Position | Function          |
| ------------- | ----------------- |
| 0             | Header (0x00)     |
| 1             | Slave Address     |
| 2             | Command           |
| 3             | Block Number      |
| 4             | Checksum          |


## Read Commands

### Read Online Values (```0x02```)
  - **Block Number:**
      - Single-Phase Inverters (NT5000): 1
      - Three-Phase Inverters (e.g. NT10000): 1-3 (one block per phase)
  - **Response:**
    | Byte Position | Function          | Conversion   | Calculation                          | Unit   |
    | ------------- | ----------------- | ------------ | ------------------------------------ | ------ |
    | 0             | UDC (DC voltage)  | to decimal   | buffer[0] * 2.8 + $udc-gradient      | V      |
    | 1             | IDC (DC current)  | to decimal   | buffer[1] * 0.08                     | A      |
    | 2             | UAC (AC voltage)  | to decimal   | buffer[2] + 100                      | V      |
    | 3             | IAC (AC current)  | to decimal   | buffer[3] * 0.12                     | A      |
    | 4             | Temperature       | to decimal   | buffer[4] - 40                       | °C     |
    | 5             | Heat Flux         | to decimal   | buffer[5] * 6                        | W/m^2  |
    | 6, 7          | Energy Today      | to decimal   | (buffer[6] * 256 + buffer[7]) / 1000 | kWh    |
    | 8, 9          | Energy Total      | to decimal   | (buffer[8] * 256 + buffer[9]) / 1000 | kWh    |
    |               | PDC (DC power)    |              | (UDC * IDC) / 1000                   | kW     |
    |               | PAC (AC power)    |              | (UAC * IAC) / 1000                   | kW     |

    **Note:** If the temperature or heat flux sensor is not connected, the buffer value is ```0x01```.

    **$udc-gradient:**
      - Single-Phase Inverters: 100
      - Three-Phase Inverters: 200


### Read Serial Number (```0x08```)
  - **Block Number:** 1 (2 is reserved for longer serial numbers) 
  - **Response:**
    | Byte Position | Function          | Conversion        |
    | ------------- | ----------------- | ----------------- |
    | 0-11          | Serial Number     | to ASCII String   |
    | 12            | Checksum          |                   |

    ```0x0D``` is a filling byte and can be ignored.


### Read Protocol + Firmware Version (```0x09```)
  - **Block Number:** 1
  - **Response:**
    | Byte Position | Function          | Conversion        |
    | ------------- | ----------------- | ----------------- |
    | 0-1           | Protocol Version  | to ASCII String   |
    | 2-11          | Firmware Version  | to ASCII String   |
    | 12            | Checksum          |                   |

    ```0x0D``` is a filling byte and can be ignored.
    

## Checksum Calculation

**Checksum = (sum of all data bytes) % 256**  
Only the checksum byte itself is excluded from the sum.
