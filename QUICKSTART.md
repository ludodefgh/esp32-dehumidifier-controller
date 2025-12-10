# Quick Start Guide

**⚡ Get your smart dehumidifier up and running in 30 minutes!**

## 🎯 Prerequisites

- [ ] All components from [BOM.md](BOM.md) 
- [ ] Soldering iron and basic tools
- [ ] ESP32-C3 flashed with ESPHome
- [ ] Home Assistant running

## 🚀 5-Step Installation

### Step 1: Build the Controller (15 min)

1. Follow [WIRING.md](WIRING.md) to assemble perfboard
2. Key points:
   - Create GND rail first
   - Adjust MP1584EN to exactly 5.0V
   - Verify PN2222 orientation (E-B-C)
3. Test with multimeter before applying power

### Step 2: Flash ESP32 (5 min)

```bash
# Copy and edit config
cd esphome
cp secrets.yaml.template secrets.yaml
nano secrets.yaml  # Add your WiFi credentials

# Flash
esphome run dehumidifier.yaml
```

### Step 3: Wire to Dehumidifier (5 min)

**Solder 2 wires to dehumidifier PCB:**
1. **LED Anode** (green wire) - where it reads 2.7V when ON
2. **Touch Pad** (blue wire) - the capacitive sensor

**Connect power:**
- 12V IN from power supply
- 12V OUT to dehumidifier

### Step 4: Test Functionality (3 min)

1. Power on system
2. Check ESPHome logs:
   ```
   - WiFi connected ✓
   - LED voltage reading ✓
   - Running state correct ✓
   ```
3. Test manual button still works
4. Test Home Assistant control

### Step 5: Integrate with Home Assistant (2 min)

Device auto-discovers! You get:
- Switch to control power
- Binary sensor for running state
- Voltage sensor for monitoring
- WiFi signal strength

## 🎨 Example Automations

### Auto-Off When Humidity Low
```yaml
automation:
  - alias: "Dehumidifier Auto Off"
    trigger:
      - platform: numeric_state
        entity_id: sensor.bathroom_humidity
        below: 50
        for: "00:10:00"
    condition:
      - condition: state
        entity_id: binary_sensor.dehumidifier_running
        state: "on"
    action:
      - service: switch.turn_off
        target:
          entity_id: switch.dehumidifier_power
```

### Notification When Full
```yaml
automation:
  - alias: "Dehumidifier Check"
    trigger:
      - platform: state
        entity_id: binary_sensor.dehumidifier_running
        to: "off"
        for: "01:00:00"
    action:
      - service: notify.mobile_app
        data:
          message: "Dehumidifier may need emptying"
```

## 🐛 Quick Troubleshooting

| Problem | Quick Fix |
|---------|-----------|
| Won't power on | Check 5V at ESP32 VIN |
| LED detection wrong | Adjust threshold in YAML (line 95) |
| Touch control doesn't work | Verify PN2222 orientation (E-B-C) |
| WiFi won't connect | Check secrets.yaml, try closer to router |
| Random resets | Check GND connections, verify 5V stable |

## 📚 Full Documentation

- [Complete README](README.md) - Overview and features
- [Bill of Materials](BOM.md) - Where to buy components
- [Wiring Guide](WIRING.md) - Detailed assembly instructions
- [Diagrams](docs/) - Visual wiring references

## 💡 Pro Tips

1. **Test each section** as you build (power, then signals)
2. **Take photos** at each stage for troubleshooting
3. **Use different wire colors** (red=power, black=GND, etc.)
4. **Label everything** with small tape/labels
5. **Double-check polarity** on capacitor before power-on

## 🆘 Need Help?

- Check [Troubleshooting in README](README.md#troubleshooting)
- Review ESPHome logs for errors
- Open a [GitHub Issue](../../issues)
- Ask in [Home Assistant Community](https://community.home-assistant.io)

## ⚠️ Safety Reminders

- Unplug before working on electronics
- Verify all voltages with multimeter
- Ensure proper insulation
- Keep away from water
- Don't leave running unattended initially

---

**Estimated total time**: 30 minutes (experienced) to 2 hours (first time)

**Difficulty**: Intermediate (basic soldering required)

**Cost**: ~$10 CAD in parts

**Result**: Full smart home control of your dehumidifier! 🎉
