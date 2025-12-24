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

