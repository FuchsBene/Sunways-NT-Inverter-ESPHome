# Communication Protocol Description

## Serial Communication Parameters

- **Baud rate:** 9600 bps
- **Data bits:** 8
- **Parity:** None
- **Stop bits:** 1
- **Slave addressing (RS485):** 1-99


## Request Structure (5 Bytes)

| Byte Position | Function          |
| ------------- | ----------------- |
| 1             | Header (0x00)     |
| 2             | Slave Address     |
| 3             | Command           |
| 4             | Block Number      |
| 5             | Checksum          |

## Read Commands


## Checksum Calculation

### Checksum = (sum of all data bytes) % 256
Only the checksum byte itself is excluded from the sum.
