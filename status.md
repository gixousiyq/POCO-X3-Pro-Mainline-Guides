# Xiaomi POCO X3 Pro linux mainline guides

## Project Status

The project is currently in a very early stage, Expect some big updates and new features added.

Current mainline kernel version is 6.10.0-rc5

#### General
- [x] Display 
- [x] CPU
- [x] UFS
- [x] USB
- [x] GPU
- [x] WiFi
- [x] Touchscreen
- [ ] Bluetooth
- [x] Battery Percentage
- [ ] Brightness control
- [x] Sdcard
- [x] Charging ```Works with everything other than xiaomi original charger, DEAD SLOW```
- [ ] Flashlight
- [ ] Fingerprint
- [ ] LTE
- [ ] SMS
- [ ] Calls
- [ ] Camera
- [ ] Microphone's
- [ ] NFC

#### Audio
- [ ] Bottom speaker
- [ ] Upper speaker
- [ ] 3.5mm

#### Sensors
- [ ] Light sensor
- [ ] Accelerometer
- [ ] Gyroscope
- [ ] Proximity
- [ ] Magnetometer
- [ ] GPS

>[!WARNING]
> WiFi and GPU and Touchscreen do not work without firmware!!!


## Known issue's 
- On boot, The screen becomes gray-blue for a couple of seconds then functions normally; No damage was noticed on the panel.
- Bluetooth is decleared and theres a button to enable it in the system, But the driver stall's when trying to power on.

## Notes
- If you see the log on the boot or shutdown being stock for a long time, Simply force reboot by long pressing the power button 
- The brightness on Mainline is the same as the last brightness set on android.
- USB Host works flawlessly and it switches automatically 
