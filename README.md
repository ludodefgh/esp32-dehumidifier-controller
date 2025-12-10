# ESP32 Smart Dehumidifier Controller

Transform any touch-button dehumidifier into a smart device controllable via Home Assistant.

![Project Status](https://img.shields.io/badge/status-working-brightgreen)
![ESPHome](https://img.shields.io/badge/ESPHome-compatible-blue)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-compatible-blue)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [Wiring Diagrams](#wiring-diagrams)
- [Installation](#installation)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project adds smart home capabilities to a basic dehumidifier (model DH-CS01 or similar) by:
- Detecting the ON/OFF state via LED sensing
- Simulating touch button presses to control power
- Integrating with Home Assistant via ESPHome

**Original Device:** DH-CS01 Dehumidifier (12V, 40W, Peltier-based)

## ✨ Features

- ✅ **State Detection**: Monitors LED status to determine if dehumidifier is running
- ✅ **Remote Control**: Turn ON/OFF via Home Assistant
- ✅ **WiFi Connectivity**: ESP32-C3 based with WiFi signal monitoring
- ✅ **Non-invasive**: Uses ESP32 alongside original electronics
- ✅ **Low Cost**: ~$8-11 CAD in components
- ✅ **Reversible**: Easy to remove and restore original functionality

## 🛠️ Hardware Requirements

### Core Components

| Component | Quantity | Notes |
|-----------|----------|-------|
| ESP32-C3 Super Mini | 1 | Main controller |
| MP1584EN Buck Converter | 1 | 12V → 5V step-down |
| PN2222 Transistor (TO-92) | 1 | Touch button simulator |
| 470µF 16V Electrolytic Capacitor | 1 | Power supply filtering |
| 10kΩ Resistors | 3 | LED divider + transistor pull-down |
| 1kΩ Resistor | 1 | Transistor base current limiter |
| Perfboard (~4.5 x 5cm) | 1 | Component mounting |
| JST-XH 2-pin Connectors | 4 pairs | Power, LED, Touch connections |
| 22-24 AWG Wire | Various colors | Red, Black, Green, Blue recommended |

### Optional Components

- Heat shrink tubing
- Pin headers (for removable ESP32)
- Kapton tape (for insulation)

### Tools Required

- Soldering iron
- Multimeter
- Wire strippers
- Cutting mat and hobby knife

## 📐 Wiring Diagrams

### System Overview

```
┌─────────────┐
│ 12V Power   │
│   Supply    │
└──────┬──────┘
       │
       ├────────────────┐
       │                │
   ┌───▼────┐      ┌────▼────────┐
   │ESP32-C3│◄────►│Dehumidifier │
   │ Board  │      │    PCB      │
   └────────┘      └─────────────┘
       │ GPIO3: LED Sense
       │ GPIO5: Touch Control
```

### Power Circuit

```
12V IN ─┬─► MP1584EN IN+ 
        │        │
        │        ├─► ESP32-C3 VIN (5V)
        │        │
        │        └─► 470µF Cap (+)
        │
        └─► Dehumidifier 12V+

GND ────┴─► Common Ground Rail
           (all GND connected)
```

### LED Detection Circuit

```
LED Anode (2.7V when ON)
    │
   10kΩ ──┬─► ESP32 GPIO3 (ADC)
          │
         10kΩ
          │
         GND
```

### Touch Control Circuit

```
ESP32 GPIO5 ──► 1kΩ ──┬─► PN2222 Base
                       │
                      10kΩ (pull-down)
                       │
                      GND

PN2222 Collector ──► Touch Pad (capacitive sensor)
PN2222 Emitter ──► GND
```

## 🔧 Installation

### Step 1: Prepare the ESP32 Board

1. Solder all components to perfboard following the layout (see `docs/perfboard-layout.png`)
2. Create a continuous GND rail on the copper side
3. **IMPORTANT**: Adjust MP1584EN output to exactly 5.0V before connecting ESP32

### Step 2: Flash ESPHome Firmware

1. Copy `esphome/dehumidifier.yaml` to your ESPHome config directory
2. Update WiFi credentials in `secrets.yaml`:
```yaml
wifi_ssid: "YourSSID"
wifi_password: "YourPassword"
```
3. Flash the ESP32:
```bash
esphome run dehumidifier.yaml
```

### Step 3: Wire to Dehumidifier

**Solder points on dehumidifier PCB:**
1. **LED Anode**: Solder green wire to positive pad of status LED
2. **Touch Pad**: Solder blue wire to capacitive touch sensor pad
3. **12V/GND**: Connect power passthrough

### Step 4: Test

1. Power up the system
2. Check ESPHome logs for proper operation
3. Verify LED voltage readings (~2.7V when ON, ~0V when OFF)
4. Test manual touch button - should still work
5. Test Home Assistant control

## ⚙️ Configuration

### ESPHome YAML

See `esphome/dehumidifier.yaml` for complete configuration.

**Key parameters to adjust:**

```yaml
sensor:
  - platform: adc
    pin: GPIO3
    filters:
      - multiply: 2.0  # Adjust if using different voltage divider

binary_sensor:
  - platform: template
    lambda: |-
      if (id(dehum_voltage).state > 1.0) {  # Adjust threshold
        return true;
      }
```

### Home Assistant Integration

The device will auto-discover in Home Assistant. You'll get:
- `switch.dehumidifier_power` - ON/OFF control
- `binary_sensor.dehumidifier_running` - State detection
- `sensor.dehumidifier_led_voltage` - Raw voltage reading
- `sensor.wifi_signal` - WiFi RSSI

## 🐛 Troubleshooting

### LED Detection Not Working

**Problem**: LED voltage always reads 0V or incorrect values

**Solutions**:
1. Verify voltage divider: Should read ~1.35V at GPIO3 when LED is ON
2. Check solder connection to LED anode
3. Confirm GPIO3 is configured as ADC input
4. Test with multimeter: LED anode should show 2.7V when ON

### Touch Control Not Working

**Problem**: Switch activates in HA but dehumidifier doesn't respond

**Solutions**:
1. Verify GPIO5 goes HIGH (3.3V) when switch activates
2. Check PN2222 orientation (E-B-C from left to right, flat side facing you)
3. Confirm touch pad solder point is correct (test by manually touching pad)
4. Remove series resistor if present - direct connection works better
5. Increase pulse duration from 200ms to 500ms in YAML

### WiFi Connection Issues

**Problem**: ESP32 won't connect or keeps disconnecting

**Solutions**:
1. Check WiFi credentials in `secrets.yaml`
2. Verify 5V power supply is stable (measure with multimeter)
3. Move closer to WiFi router during initial setup
4. Check that 470µF capacitor is properly connected
5. Review ESPHome logs for specific error messages

### Power Issues

**Problem**: ESP32 doesn't boot or resets randomly

**Solutions**:
1. Verify MP1584EN output is exactly 5.0V (adjust potentiometer)
2. Check all GND connections are solid
3. Ensure 470µF capacitor is connected with correct polarity
4. Measure current draw - should be <250mA normally

## 📊 Technical Specifications

- **Operating Voltage**: 12V DC input
- **ESP32 Supply**: 5V @ ~150-250mA
- **Dehumidifier Power**: 40W (12V @ 3.3A)
- **GPIO Logic Level**: 3.3V
- **LED Detection Range**: 0-2.7V (scaled to 0-1.35V via divider)
- **Touch Pulse Duration**: 200-500ms
- **WiFi Update Interval**: 60s
- **LED Voltage Update Interval**: 1s

## 📸 Photos

See `docs/photos/` directory for:
- Assembled perfboard
- Dehumidifier PCB solder points
- Final installation
- Wiring detail shots

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

**Areas for improvement:**
- Support for other dehumidifier models
- 3D printed enclosure design
- Alternative sensing methods
- Additional automation examples

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- ESPHome community for excellent documentation
- Home Assistant for smart home platform
- All contributors and testers

## 📞 Support

If you encounter issues:
1. Check the [Troubleshooting](#troubleshooting) section
2. Review ESPHome logs for error messages
3. Open an issue on GitHub with:
   - Hardware details
   - ESPHome configuration
   - Log output
   - Photos of your setup

---

**⚠️ Disclaimer**: This project involves modifying electrical devices. Work carefully and at your own risk. Ensure all connections are insulated and secure. Unplug devices before working on them.

**Made with ❤️ for the Home Assistant community**
