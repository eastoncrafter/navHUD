# Building navHUD

This document provides instructions for building both components of the navHUD project:
- The Android application (navHUD)
- The ESP32 firmware (navHUD_esp)

## Building the Android Application

### Prerequisites

- **Java Development Kit (JDK)**: JDK 11 or higher
- **Android SDK**: Android SDK with API level 33+ (minimum) and 36 (target)
- **Git**: For cloning the repository

### Build Instructions

1. **Clone the repository** (if you haven't already):
   ```bash
   git clone https://github.com/eastoncrafter/navHUD.git
   cd navHUD
   ```

2. **Navigate to the Android project directory**:
   ```bash
   cd navHUD
   ```

3. **Build the application**:
   
   On Linux/macOS:
   ```bash
   ./gradlew assembleDebug
   ```
   
   On Windows:
   ```bash
   gradlew.bat assembleDebug
   ```

4. **The built APK will be located at**:
   ```
   navHUD/app/build/outputs/apk/debug/app-debug.apk
   ```

### Building a Release Version

To build a release version:

```bash
./gradlew assembleRelease
```

The release APK will be at:
```
navHUD/app/build/outputs/apk/release/app-release-unsigned.apk
```

### Installing on a Device

To build and install directly to a connected Android device:

```bash
./gradlew installDebug
```

### Cleaning the Build

To clean previous build artifacts:

```bash
./gradlew clean
```

## Building the ESP32 Firmware

### Prerequisites

- **Python**: Python 3.6 or higher
- **PlatformIO**: Install via pip or as an IDE extension
- **USB Drivers**: Appropriate USB drivers for your ESP32 board

### Installing PlatformIO

Install PlatformIO Core via pip:

```bash
pip install platformio
```

Or install the PlatformIO IDE extension for VS Code or Atom.

### Build Instructions

1. **Navigate to the ESP32 project directory**:
   ```bash
   cd navHUD_esp
   ```

2. **Build the firmware**:
   ```bash
   pio run
   ```

3. **The compiled firmware will be located in**:
   ```
   navHUD_esp/.pio/build/esp32-c3-devkitm-1/
   ```

### Uploading to ESP32

1. **Connect your ESP32-C3 DevKit to your computer via USB**

2. **Upload the firmware**:
   ```bash
   pio run --target upload
   ```

   PlatformIO will automatically detect the USB port in most cases.

3. **To specify a port manually**:
   ```bash
   pio run --target upload --upload-port /dev/ttyUSB0  # Linux/macOS
   pio run --target upload --upload-port COM3           # Windows
   ```

### Monitoring Serial Output

To view serial output from the ESP32:

```bash
pio device monitor
```

Or combined upload and monitor:

```bash
pio run --target upload && pio device monitor
```

### Cleaning the Build

To clean previous build artifacts:

```bash
pio run --target clean
```

## Troubleshooting

### Android Build Issues

**Issue**: `SDK location not found`
- **Solution**: Create a `local.properties` file in the `navHUD` directory with:
  ```
  sdk.dir=/path/to/your/Android/Sdk
  ```

**Issue**: `Gradle sync failed`
- **Solution**: Ensure you have a stable internet connection for dependency downloads
- Try: `./gradlew --refresh-dependencies`

**Issue**: `Unsupported class file major version`
- **Solution**: Ensure you're using JDK 11 or higher

### ESP32 Build Issues

**Issue**: `Could not find the main PlatformIO library`
- **Solution**: Reinstall PlatformIO: `pip install --upgrade platformio`

**Issue**: `USB port not found during upload`
- **Solution**: 
  - Ensure the ESP32 is properly connected
  - Install appropriate USB drivers (CP210x or CH340 depending on your board)
  - Check if the device appears: `pio device list`

**Issue**: `Permission denied on /dev/ttyUSB0` (Linux)
- **Solution**: Add your user to the dialout group:
  ```bash
  sudo usermod -a -G dialout $USER
  ```
  Then log out and log back in.

**Issue**: Library dependencies not found
- **Solution**: PlatformIO should automatically download dependencies. If issues persist:
  ```bash
  pio pkg install
  ```

## Hardware Requirements

### For Android Application
- Android device running Android 13 (API 33) or higher
- Bluetooth Low Energy (BLE) support

### For ESP32 Firmware
- ESP32-C3 DevKitM-1 board (or compatible ESP32 board)
- 128x64 OLED display module (I2C interface)
- USB cable for programming and power

## Additional Resources

- [Android Developer Documentation](https://developer.android.com/)
- [PlatformIO Documentation](https://docs.platformio.org/)
- [ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/)
