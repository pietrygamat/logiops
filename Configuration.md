# DISCALIMER: The configuration file is a work in progress and is subject to change.

Logid uses a standard libconfig-style config file stored in `/etc/logid.cfg` by default (although you launch logid with the -c option to change it).

`:` and `=` are used for defining variables and are interchangeable. (e.g. `name: "foo";` is the same as `name = "foo";`)

# Defining Devices
Devices are defined in an array field called `devices`. This array consists of objects that define a device.

e.g. `devices: ( { device object }, { device object } ... );`

## Device Object
The following are a list of fields that a device object contains.
### name
This is a required string field that defines the name of the device. To get the name of the device, launch logid with the device connected and it should print out a message with the device name. (e.g. `name: "MX Master";`)

### buttons
This is an optional array field that defines the mappings for buttons.

e.g. `buttons: ( { button object }, { button object } ... );`

#### Button Objects
##### cid
This is a required integer field that defines the Control ID of the button that is being remapped. (e.g. `cid: 0xc4;`)
##### action
This is a required object field that defines the new action of the button. (e.g. `action: { ... };` )

Refer to the actions section for more information.

### dpi
This is an optional integer field that defines the DPI for a mouse that supports adjustable DPI. (e.g. `dpi: 1000;`)

### smartshift
This is an optional object field that defines the SmartShift settings for a mouse that supports it.

```
smartshift:
{
    on: true;
    threshold: 30;
    default_threshold: 30;
};
```

#### on
This is an optional boolean field that defines whether SmartShift should be activated. (`true` for SmartShift, `false` for free-spin).

#### threshold
This is an optional integer field between 1-255 that defines the threshold required to change the SmartShift wheel to free-spin.

#### default_threshold
This is an optional integer field between 1-255 that defines the mouse's default threshold required to change the SmartShift wheel to free-spin.

### hiresscroll
This is an optional object field that defines the HiRes mouse-scrolling settings for a device that supports it.

```
hiresscroll:
{
    hires: true;
    invert: false;
    target: false;
};
```
#### hires
This is an optional boolean field that defines whether the mouse wheel should be high resolution or not.

#### invert
This is an optional boolean field that defines whether to invert the mouse wheel.

#### target
This is an optional boolean field that defines whether mousewheel events should send as an HID++ notification or work normally (`true` for HID++ notification, `false` for normal usage).

# Actions
### type
This is a required string field that defines the type of action. (e.g. `type: "None";`). The following is a list of possible actions with their additional fields.

## None
This does nothing. There are no additional fields.

## Keypress
This maps press and release events of a button to a list of keys/buttons.
### keys
This is a required string array field that defines the keys to be pressed/released. For a list of key/button strings, refer to [linux/input-event-codes.h](https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h). (e.g. `keys: ["KEY_A", "KEY_B"];`)

## Gestures
This action disables mouse movement while the button is pressed and allows you to assign actions for each direction. The possible directions are `Up`, `Down`, `Left`, `Right`, and `None`.

### gestures
This is a required array of gesture objects that map a direction to a gesture mode and an action.

e.g. `gestures: ( { gesture object }, { gesture object } ... );`

#### direction
This is a required string field in a gesture object that defines the direction of the gesture. (e.g. `direction: "Up"`)

#### mode
This is an optional string field that defines the mode of the gesture. This field defaults to `OnRelease` if it is omitted.

The following is a list of gesture modes:

##### NoPress
This mode does nothing. The `action` field is ignored when this mode is used.
##### OnRelease
This mode presses and releases an action when the gesture button is released
##### OnFewPixels
This mode presses and releases an action after the mouse is moved every n pixels (where n is the integer field `pixels`).
##### Axis
This mode maps a gesture movement to an axis. The axis is defined as a string (e.g. "REL_WHEEL") in the `axis` field and the multiplier for its movement is defined in the `axis_multiplier` field. For a list of axis strings, refer to [linux/input-event-codes.h](https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h). 

#### action
This is a mandatory field that defines the action the gesture uses. This can be any action other than `Gestures`. Refer to the entire Actions section for more details.

## ToggleSmartShift
This action toggles the SmartShift scrolling feature when pressed. There are no additional fields.

## ToggleHiresScroll
This action toggles high resolution scrolling when pressed. There are no additional fields.

## CycleDPI
This action cycles between the given DPIs.

### dpis
This is a mandatory integer array field that defines what DPIs to cycle through. (e.g. `dpis: [800, 1000, 1200];`)

## ChangeDPI
This action increments the DPI by the value given in the `inc` field.

### inc
This is an integer array field that defines what to increase the DPI by.

