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
**Returns:** Byte 0: Number of features

### Function 1: Get feature
**Arguments:** Byte 0: Feature index

**Returns:** Byte 0: Feature ID MSB; Byte 1: Feature ID LSB

## 0x0002: Feature info
#### Function 0: Get base feature?
If the feature ID is a newer version of another feature, it should return the feature ID of its base version.

**Arguments:** Byte 0: Feature ID MSB; Byte 1: Feature ID LSB

**Returns:** Byte 0: Base Feature ID MSB; Byte 1: Base Feature ID LSB

## 0x0003: Device FW version
### Function 0: Get number of firmwares
**Returns:** Byte 0: Number of firmwares; Byte 1-4: Unit UUID; Byte 5: Protocol Support MSB (reserved); Byte 6: Protocol Support (Lower nibble bits are: USB, Unifying, BTLE, Bluetooth)

### Function 1: Get firmware version
**Arguments:** Byte 0: Firmware index

**Returns:** Byte 0: Firmware type; Byte 1-3: ASCII Firmware Prefix; Byte 4: Firmware Number; Byte 5: Revision; Byte 6-7: Build Number; Byte 8: Active entity; Byte 9-10: PID; Byte 11-15: Extra versioning info

Firmware types: 0 = Main; 1 = Bootloader; 2 = Hardware

## 0x0005: Device name
### Function 0: Get length
**Returns:** Byte 0: Length of device name

### Function 1: Get device name
**Arguments:** Byte 0: Character index

**Returns:** 16 bytes of the device name as a null-terminated string starting from the given index

## 0x0020: Reset
#### Function 0: Nothing?
#### Function 1: Reset to default settings

## 0x0021: Crypto identifier
#### Function 0: Returns part 1 of crypto identifier?
#### Function 1: Returns part 2 of crypto identifier?
#### Function 2: Generates new crypto identifier?