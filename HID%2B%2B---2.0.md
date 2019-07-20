# Features
## 0x0000: Root
_This feature ID on the device will always be 0._
### Function 0: Get Feature
**Arguments:** Byte 0: Feature ID MSB; Byte 1: Feature ID LSB

**Returns:** Byte 0: Feature Index; Byte 1: Flags (OHE-----) where O is obsolete, H is hidden, and E is engineering; Byte 2: Feature version

### Function 1: Get Protocol Version
**Arguments:** Byte 0 & 1: Zero for padding; Byte 2: Ping data (to be returned)

**Returns:** Byte 0: Protocol Version (Major); Byte 1: Target SW; Byte 2: Ping data

**Target SW:** Bit 7: SW defined by feature 0x0030; Bit 3-6 are unused; Bit 2: Preference Manager; Bit 1: Gaming Software; Bit 0: Device Manager

## 0x0001: Feature set
### Function 0: Get number of features

**Arguments:** None

**Returns:** Byte 0: Number of features

### Function 1: Get feature

**Arguments:** Byte 0: Feature index

**Returns:** Byte 0: Feature ID MSB; Byte 1: Feature ID LSB