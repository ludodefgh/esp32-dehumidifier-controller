# Bill of Materials (BOM)

## Complete Parts List

This document lists all components needed to build the ESP32 Smart Dehumidifier Controller.

## Core Electronics Components

| Item | Component | Specification | Quantity | Unit Price (CAD) | Total (CAD) | Notes |
|------|-----------|---------------|----------|------------------|-------------|-------|
| 1 | ESP32-C3 Super Mini | WiFi/BLE, USB-C | 1 | $3.00 - $5.00 | $5.00 | Main microcontroller |
| 2 | MP1584EN Buck Converter | 12V→5V, Mini module | 1 | $1.00 - $2.00 | $1.50 | Step-down voltage regulator |
| 3 | PN2222 Transistor | NPN, TO-92 package | 1 | $0.10 | $0.10 | Touch button simulator |
| 4 | Electrolytic Capacitor | 470µF, 16V or higher | 1 | $0.20 | $0.20 | Power supply filtering |
| 5 | Resistor | 10kΩ, 1/4W | 3 | $0.05 | $0.15 | Voltage divider + pull-down |
| 6 | Resistor | 1kΩ, 1/4W | 1 | $0.05 | $0.05 | Base current limiter |

**Subtotal Electronics: ~$7.00 CAD**

## Connection Components

| Item | Component | Specification | Quantity | Unit Price (CAD) | Total (CAD) | Notes |
|------|-----------|---------------|----------|------------------|-------------|-------|
| 7 | JST-XH Connector | 2-pin, male (PCB mount) | 4 | $0.25 | $1.00 | For perfboard |
| 8 | JST-XH Connector | 2-pin, female with wire | 4 | $0.30 | $1.20 | For cables |
| 9 | Wire | 22-24 AWG, stranded | 2m | $0.50/m | $1.00 | Multiple colors |
| 10 | Perfboard | Single-sided, 5x7cm | 1 | $1.00 | $1.00 | Component mounting |
| 11 | Solder | 60/40 or lead-free | - | - | $0.50 | For assembly |

**Subtotal Connections: ~$4.70 CAD**

## Optional Components

| Item | Component | Specification | Quantity | Unit Price (CAD) | Notes |
|------|-----------|---------------|----------|------------------|-------|
| 12 | Pin Headers | 2.54mm, female | 2x8 pins | $0.50 | For removable ESP32 |
| 13 | Heat Shrink Tubing | Assorted sizes | 10cm | $0.20 | Cable insulation |
| 14 | Kapton Tape | Heat-resistant | Small roll | $2.00 | Component insulation |
| 15 | Standoffs | M3, 10mm | 4 | $0.40 | Board mounting |

**Subtotal Optional: ~$3.10 CAD**

## Total Project Cost

| Category | Cost (CAD) |
|----------|------------|
| Core Electronics | $7.00 |
| Connection Components | $4.70 |
| Optional Components | $3.10 |
| **TOTAL (without optional)** | **$11.70** |
| **TOTAL (with optional)** | **$14.80** |

*Prices are approximate and based on online retailers (AliExpress, Amazon, DigiKey) as of 2024.*

## Where to Buy

### Online Retailers (Canada)

**Budget Option (2-4 weeks shipping):**
- [AliExpress](https://www.aliexpress.com) - Cheapest for bulk components
- [Banggood](https://www.banggood.com) - Good selection of modules

**Fast Shipping (1-3 days):**
- [Amazon.ca](https://www.amazon.ca) - ESP32, modules, connectors
- [DigiKey.ca](https://www.digikey.ca) - Resistors, capacitors, transistors
- [Mouser.ca](https://www.mouser.ca) - Professional components

**Local (Montreal area):**
- Addison Electronics - Downtown Montreal
- Creatron Inc. - Various electronic components

### Recommended Kits/Bundles

If buying components, consider these kits to have extras:

1. **Resistor Kit**: 1/4W assortment (10Ω - 1MΩ) - ~$10-15
   - Covers all resistor needs + spares
   
2. **Capacitor Kit**: Electrolytic assortment - ~$15-20
   - Multiple values for future projects

3. **Transistor Kit**: Common NPN/PNP types - ~$10
   - PN2222, 2N3904, 2N2222, etc.

4. **JST Connector Kit**: Mixed sizes - ~$15-20
   - XH, PH, SH connectors with crimping tool

## Component Specifications Detail

### ESP32-C3 Super Mini

**Technical Specs:**
- MCU: ESP32-C3 (RISC-V, 160MHz)
- Flash: 4MB
- RAM: 400KB
- WiFi: 802.11 b/g/n
- Bluetooth: BLE 5.0
- GPIO: 13 available
- ADC: 6 channels, 12-bit
- USB: Native USB (Type-C)
- Dimensions: ~23 x 18 mm

**Why this model?**
- Native USB for easy programming
- Small form factor
- Built-in voltage regulator (3.3V)
- Low cost
- Good ESPHome support

### MP1584EN Buck Converter

**Technical Specs:**
- Input: 4.5V - 28V DC
- Output: 0.8V - 20V DC (adjustable)
- Current: 3A max
- Efficiency: ~96%
- Switching frequency: 1.5MHz
- Dimensions: ~22 x 17 x 4 mm

**Setup:**
- Adjust potentiometer to exactly 5.0V output
- Use multimeter to verify before connecting ESP32

### PN2222 Transistor

**Technical Specs:**
- Type: NPN Bipolar Junction Transistor (BJT)
- Package: TO-92
- Collector-Emitter Voltage (Vceo): 40V
- Collector Current (Ic): 600mA continuous
- DC Current Gain (hFE): 100-300
- Power Dissipation: 625mW

**Pinout (TO-92, flat side facing you):**
```
  ___
 /   \
|E B C|
 \___/
  |||
  123
```
1. Emitter (E)
2. Base (B)
3. Collector (C)

**Alternatives:**
- 2N2222 (metal can, TO-18)
- 2N3904 (similar specs, TO-92)
- BC547 (lower current, 100mA)

### Resistors

**Specifications:**
- Type: Carbon film or metal film
- Tolerance: ±5% (gold band) or ±1% (brown band)
- Power: 1/4W (0.25W) minimum
- Temperature coefficient: 100 ppm/°C typical

**Color Codes:**
- **10kΩ**: Brown-Black-Orange-Gold
- **1kΩ**: Brown-Black-Red-Gold

### Capacitor 470µF

**Specifications:**
- Type: Aluminum electrolytic
- Capacitance: 470µF
- Voltage rating: 16V minimum (25V recommended)
- Tolerance: ±20% typical
- ESR: Low (<1Ω at 100kHz)
- Temperature: -40°C to +105°C

**⚠️ IMPORTANT:**
- This is a **polarized** component
- Negative leg is marked with stripe on body
- **Must** connect + to 5V, - to GND
- Incorrect polarity = capacitor explosion!

### JST-XH Connectors

**Specifications:**
- Series: JST-XH
- Pitch: 2.5mm
- Pins: 2-pin
- Current rating: 3A per pin
- Voltage rating: 250V AC/DC
- Wire gauge: 22-28 AWG

**Types needed:**
- **Male (PCB mount)**: Solder to perfboard
- **Female (with wire)**: For cable assemblies

**Crimping:**
- Use JST XH crimping tool
- Or carefully crimp with small pliers
- Ensure good mechanical and electrical connection

## Storage and Organization

**Recommended storage:**
- Small parts organizer boxes
- Label each compartment
- Keep resistors sorted by value
- Store connectors with mating pairs

**Labeling resistors:**
- Use multimeter to verify values
- Small adhesive labels work well
- Group by decade (1k, 10k, 100k, etc.)

## Quality Considerations

**When buying:**
- ✅ Check seller ratings
- ✅ Read reviews carefully
- ✅ Verify specifications match
- ✅ Look for genuine parts (especially ESP32)
- ⚠️ Be cautious of "too cheap" deals
- ⚠️ Counterfeit ESP32 boards exist

**Testing new components:**
- Test MP1584EN output voltage before use
- Verify resistor values with multimeter
- Check capacitor polarity markings
- Test transistor with diode function on multimeter

## Reusability

Most components can be reused for other projects:
- ESP32-C3 can be reprogrammed
- MP1584EN useful for any 5V project
- Resistors, capacitors, transistors universal
- JST connectors modular

**Expected lifetime:**
- ESP32-C3: 10+ years
- MP1584EN: 5+ years (depends on heat)
- Passive components: 20+ years
- Connectors: Hundreds of insertions

## Environmental Notes

**Disposal:**
- Electronics should NOT go in regular trash
- Find local e-waste recycling center
- Some retailers accept old electronics
- Capacitors may contain electrolyte

**Lead-free option:**
- Use lead-free solder (SAC305)
- Slightly higher melting point
- Better for environment
- Required in some jurisdictions

---

**Last Updated**: December 2024

**Questions?** Open an issue on GitHub or check the main README.md
