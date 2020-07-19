# Proposed DBus Interface for logid

This is still a draft and is subject to change.

DBus name pizza.pixl.LogiOps

```
/pizza/pixl/logiops
├ pizza.pixl.LogiOps.Configuration
|  ├ reload() (method, void)
|  └ configFile (read, string)
├ pizza.pixl.LogiOps.DeviceManager
|  ├ enumerate() (method, void)
|  ├ deviceAdded() (signal, string)
|  ├ deviceRemoved() (signal, string)
|  └ devices (read, string[])
├ /pizza/pixl/logiops/device/<nickname>
|  ├ pizza.pixl.LogiOps.Device
|  |  ├ reconfigure() (method, void)
|  |  ├ reloadConfig() (method, void)
|  |  ├ saveConfig() (method, void)
|  |  ├ sleep() (signal, void)
|  |  ├ wakeup() (signal, void)
|  |  ├ rawPath (read, string)
|  |  ├ deviceIndex (read, byte)
|  |  ├ name (read, string)
|  |  ├ pid (read, uint16)
|  |  ├ asleep (read, boolean)
|  |  ├ supportedFeatures (read, string[])
|  |  └ receiver (read, string) // empty if not a receiver device
|  ├ /pizza/pixl/logiops/device/<nickname>/buttons/<cid>
|  |  ├ pizza.pixl.LogiOps.Device.Button
|  |  |  ├ remapButton(string actionType) (method, void)
|  |  |  ├ actionType (read, string)
|  |  |  ├ flags (read, string[])
|  |  ├ /pizza/pixl/logiops/device/<nickname>/buttons/<cid>/gesture/<direction>
|  |  |  ├ pizza.pixl.LogiOps.Action.Gesture
|  |  |  |  ├ setMode(string mode) (method, void)
|  |  |	 |  ├ setAction(string, actionType) (method, void)
|  |  |  |  ├ gestureMode (read, string)
|  |  |  |  ├ threshold (read/write, int16)
|  |  |  |  └ actionType (read, string)
|  |  |  ├ pizza.pixl.LogiOps.Action.Gesture.Interval
|  |  |  |  └ interval (read/write, int16)
|  |  |  ├ pizza.pixl.LogiOps.Action.Gesture.Axis
|  |  |  |  ├ multiplier (read/write, double)
|  |  |  |  └ axis (read/write, uint32)
|  |  └ /pizza/pixl/logiops/device/<nickname>/buttons/<cid>[/gesture/<direction>]/action
|  |     ├ pizza.pixl.LogiOps.Action.ChangeDPI
|  |     |  ├ interval (read/write, int16)
|  |     |  └ sensor (read/write, byte)
|  |     ├ pizza.pixl.LogiOps.Action.ChangeHost
|  |     |  ├ offsetMode (read/write, bool)
|  |     |  └ host (read/write, byte)
|  |     ├ pizza.pixl.LogiOps.Action.CycleDPI
|  |     |  ├ dpis (read/write, int16[])
|  |     |  └ sensor (read/write, byte)
|  |     └ pizza.pixl.LogiOps.Action.Keypress
|  |        └ keys (read/write, uint32[])
|  ├ /pizza/pixl/logiops/device/<nickname>/dpi
|  |  └ pizza.pixl.LogiOps.DPI
|  |     ├ getDPI(byte sensor) (method, uint16)
|  |     ├ setDPI(byte sensor, uint16 dpi) (method, uint16)
|  |     ├ sensorCount (read, byte)
|  |     └ supportedDPIs (read, struct{uint16[], bool})
|  ├ /pizza/pixl/logiops/device/<nickname>/hiresscroll
|  |  └ pizza.pixl.LogiOps.HiresScroll
|  |    ├ hires (read/write, byte) (0 for default, 1 for false, 2 for true)
|  |    ├ inverted (read/write, byte)
|  |    ├ target (read/write, byte)
|  └ /pizza/pixl/logiops/device/<nickname>/smartshift
|     └ pizza.pixl.LogiOps.SmartShift
|       ├ active (read/write, bool)
|       ├ threshold (read/write, byte)
|       └ defaultThreshold (read/write, byte)
└ /pizza/pixl/logiops/receiver/<nickname>
   └ pizza.pixl.LogiOps.Receiver
     ├ pair(byte timeout) (method, void)
     ├ stopPairing() (method, void)
     ├ unpair(byte deviceIndex) (method, void)
     ├ devicePaired() (signal, string)
     ├ deviceUnpaired() (signal, string)
     └ devices (read, string[])
```

## Nickname system

I still haven't figured out this part, all I know is that I don't want the device name to refer to the raw path.