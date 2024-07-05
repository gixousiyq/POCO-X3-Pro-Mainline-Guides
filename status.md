# Xiaomi POCO X3 Pro linux mainline guides

## Project Status

The project is currently in a very early stage, Expect some big updates and new features added.

Current mainline kernel version is 6.10.0-rc5

#### General
- [x] Display 
- [x] CPU
- [x] UFS
- [x] USB
- [x] GPU ```[1]```
- [x] WiFi ```[1]```
- [ ] Touchscreen ```[4]```
- [ ] Bluetooth ```[4]```
- [ ] Battery Percentage ```[3]```
- [ ] Brightness control ```[2]```
- [x] Sdcard ```Should be working, Untested```
- [ ] Charging ```[3]```
- [ ] Flashlight ```[2]```
- [ ] Fingerprint ```[6]```
- [ ] LTE ```[4]```
- [ ] SMS ```[4]```
- [ ] Calls ```[4]```
- [ ] Camera ```[6]```
- [ ] Microphone's ```[4]```

#### Audio
- [ ] Bottom speaker ```[2]```
- [ ] Upper speaker ```[2]```
- [ ] tas256x chip ```[4]```
- [ ] 3.5mm ```[4]```

#### Sensors
- [ ] Light sensor ```[4]```
- [ ] Accelerometer ```[4]```
- [ ] Gyroscope ```[4]```
- [ ] Proximity ```[4]```
- [ ] Magnetometer ```[4]```
- [ ] GPS ```[4]```

## Statuses
[1]: Needs firmware 

[2]: Not decleared in device tree

[3]: Not decleared in device tree + No driver yet

[4]: Needs firmware + Not decleared in device tree yet

[4]: Needs firmware + driver is broken

[6]: Needs firmware + Not decleared yet in device tree + No driver yet


## Known issue's 
- On boot on huaxing panel, The screen becomes gray-blue for a couple of seconds then functions normally; No damage was noticed on the panel, Will try to fix it asap.
- Bluetooth is decleared and theres a button to enable it in the system, But the driver stall's when trying to power on.

## Notes
- If you see the log on the boot or shutdown being stock for a long time, Simply force shutdown 
- The brightness on Mainline is the same as the last brightness set on android.
- USB Host works flawlessly and it switches automatically 
