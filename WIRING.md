# Wiring Guide

Complete step-by-step wiring instructions for the ESP32 Smart Dehumidifier Controller.

## 📋 Table of Contents

- [Before You Start](#before-you-start)
- [Tools Required](#tools-required)
- [Perfboard Layout](#perfboard-layout)
- [Wiring Steps](#wiring-steps)
- [Connection Diagrams](#connection-diagrams)
- [Testing Procedures](#testing-procedures)
- [Common Mistakes](#common-mistakes)

## 🛠️ Before You Start

### Safety First

⚠️ **IMPORTANT SAFETY WARNINGS:**
- Always unplug devices before working on them
- Verify voltage levels with multimeter before connecting
- Use proper insulation on all connections
- Work in a well-ventilated area when soldering
- Keep a fire extinguisher nearby
- Wear safety glasses when cutting wires or components

### Preparation Checklist

- [ ] Read through entire guide before starting
- [ ] Gather all components (see BOM.md)
- [ ] Prepare clean, well-lit workspace
- [ ] Test multimeter on known voltage source
- [ ] Organize components in labeled containers
- [ ] Have schematic/layout printed or on second screen

## 🔧 Tools Required

### Essential Tools

1. **Soldering iron** (25-40W, temperature controlled recommended)
2. **Solder** (60/40 or lead-free)
3. **Multimeter** (with continuity, voltage, and resistance functions)
4. **Wire strippers** (22-24 AWG)
5. **Flush cutters** (for component leads)
6. **Needle-nose pliers**
7. **Tweezers** (for holding small components)

### Helpful Tools

- Helping hands / PCB holder
- Solder sucker / desoldering braid
- Flux pen
- Isopropyl alcohol (for cleaning)
- Anti-static mat
- Magnifying glass / loupe

## 📐 Perfboard Layout

### Component Placement (Top View)

```
┌──────────────────────────────────────┐
│                                      │
│  470µF        MP1584EN               │
│   Cap                                │
│    │                                 │
│  ──┴──      ┌────────┐               │
│  │   │      │12V → 5V│               │
│  └───┘      └────────┘               │
│                                      │
│         ┌──────────────┐             │
│         │  ESP32-C3    │             │
│         │ Super Mini   │             │
│         └──────────────┘             │
│                                      │
│   LED Voltage Divider   Touch Ctrl  │
│                                      │
│      R1        PN2222                │
│     10kΩ       (E-B-C)               │
│      │           │                   │
│      ├─GPIO3     R2                  │
│      │          1kΩ─GPIO5            │
│      R3          │                   │
│     10kΩ         R4                  │
│      │          10kΩ                 │
│     GND         GND                  │
│                                      │
│  [GND RAIL]═══════════════════════   │
│                                      │
└──────────────────────────────────────┘
```

### Copper Side (Bottom View)

The copper side should have:
- One continuous GND rail (thick solder trace or bare wire)
- Power traces from MP1584EN to ESP32
- Signal traces from resistors to ESP32 GPIO pins
- All traces kept as short as practical

## 🔌 Wiring Steps

### Step 1: Prepare the Perfboard

1. **Cut perfboard to size** (approximately 15 x 20 holes)
2. **Clean the board** with isopropyl alcohol
3. **Mark GND rail location** on copper side (typically bottom row)
4. **Plan component positions** using layout diagram

### Step 2: Create GND Rail (Copper Side)

**Method A: Solder Trace**
1. Apply flux along entire GND rail row
2. Heat first pad and apply generous solder
3. Drag iron along pads, adding solder as needed
4. Create continuous thick trace
5. Test continuity with multimeter

**Method B: Bare Wire**
1. Cut 18-20 AWG bare copper wire to length
2. Tin the wire with solder
3. Solder wire along GND rail pads
4. Test continuity

### Step 3: Install Power Components

**3.1 MP1584EN Module**

```
Position: Top-right area
Orientation: Adjust potentiometer accessible
Pins: IN+, IN-, OUT+, OUT-
```

1. Insert MP1584EN into perfboard
2. Solder all four pins
3. Test that module is secure

**3.2 470µF Capacitor**

```
Position: Top-left, near MP1584EN
Polarity: Negative stripe toward GND rail
```

1. **CRITICAL**: Identify negative lead (shorter, marked with stripe)
2. Bend leads to match perfboard spacing
3. Insert with positive lead toward 5V
4. Solder both leads
5. Trim excess lead length
6. **Verify polarity** before proceeding!

### Step 4: Install ESP32-C3

**Option A: Direct Solder (Permanent)**
1. Place ESP32 face-up in center area
2. Solder all pins on copper side
3. Ensure USB port is accessible

**Option B: Socket (Removable) - RECOMMENDED**
1. Solder female pin headers to perfboard
2. Insert ESP32 into headers
3. ESP32 can be removed for programming

### Step 5: Install Resistors

**5.1 LED Voltage Divider**

```
R1: 10kΩ (Brown-Black-Orange)
R2: 10kΩ (Brown-Black-Orange)
Position: Left side, vertical
```

1. **R1 (top resistor)**:
   - Top lead: Will connect to LED+ wire
   - Bottom lead: Junction with R2 and GPIO3

2. **R2 (bottom resistor)**:
   - Top lead: Junction with R1 and GPIO3
   - Bottom lead: To GND rail

3. Solder both resistors vertically
4. On copper side, connect junction to GPIO3

**5.2 Transistor Control Resistors**

```
R3: 1kΩ (Brown-Black-Red)
R4: 10kΩ (Brown-Black-Orange)
Position: Lower area, near transistor
```

1. **R3**: GPIO5 → PN2222 Base (1kΩ)
2. **R4**: PN2222 Base → GND (10kΩ pull-down)

### Step 6: Install PN2222 Transistor

**CRITICAL: Verify Pinout!**

```
Front view (flat side facing you):
    ___
   /   \
  |E B C|
   \___/
    |||
    123

E = Emitter (to GND)
B = Base (from GPIO5 via 1kΩ)
C = Collector (to Touch pad)
```

**Installation:**
1. Identify flat side of transistor
2. Insert with flat side toward you
3. From left to right: E-B-C
4. Solder all three pins
5. **Double-check orientation** before proceeding

### Step 7: Add JST Connectors

**Connector Locations (Component Side):**

```
J1: 12V IN    (top left)
J2: 12V OUT   (top right)
J3: LED       (left side)
J4: TOUCH     (lower area)
```

**For each connector:**
1. Insert male JST connector pins into perfboard
2. Ensure polarity marking visible
3. Solder both pins on copper side
4. Label each connector

### Step 8: Wire Power Connections (Copper Side)

**8.1 12V Input**
```
J1 (12V IN) Pin 1 (+) → MP1584EN IN+
J1 (12V IN) Pin 2 (-) → GND Rail
```

**8.2 12V Passthrough**
```
J1 (12V IN) Pin 1 (+) → J2 (12V OUT) Pin 1 (+)
J1 (12V IN) Pin 2 (-) → J2 (12V OUT) Pin 2 (-)
```
Use thicker wire (20-22 AWG) for 12V connections carrying dehumidifier current.

**8.3 5V Distribution**
```
MP1584EN OUT+ → ESP32 VIN
MP1584EN OUT+ → 470µF Cap (+)
MP1584EN OUT- → GND Rail
470µF Cap (-) → GND Rail
```

### Step 9: Wire Signal Connections (Copper Side)

**9.1 LED Detection**
```
J3 Pin 1 (LED+) → R1 (top 10kΩ) top lead
R1 bottom / R2 top junction → ESP32 GPIO3
R2 bottom → GND Rail
J3 Pin 2 (GND) → GND Rail
```

**9.2 Touch Control**
```
ESP32 GPIO5 → R3 (1kΩ) → PN2222 Base
PN2222 Base → R4 (10kΩ) → GND Rail
PN2222 Emitter → GND Rail
PN2222 Collector → J4 Pin 1 (TOUCH)
J4 Pin 2 (GND) → GND Rail
```

### Step 10: Final Assembly

1. **Trim excess wire and component leads**
2. **Apply heat shrink** to exposed solder joints if desired
3. **Clean flux residue** with isopropyl alcohol
4. **Label all connectors** clearly
5. **Take photos** for documentation

## 🔍 Testing Procedures

### Test 1: Visual Inspection

- [ ] All solder joints shiny and smooth (not cold/cracked)
- [ ] No solder bridges between adjacent pads
- [ ] All component leads trimmed
- [ ] Correct polarity on capacitor
- [ ] PN2222 oriented correctly (E-B-C)
- [ ] All connectors firmly attached

### Test 2: Continuity Tests (Power OFF)

**GND Rail:**
```
Test all GND points:
- J1 Pin 2 to GND Rail: Should beep
- J2 Pin 2 to GND Rail: Should beep
- J3 Pin 2 to GND Rail: Should beep
- J4 Pin 2 to GND Rail: Should beep
- ESP32 GND to GND Rail: Should beep
- All should beep to each other
```

**Power Traces:**
```
- J1 Pin 1 to J2 Pin 1: Should beep (12V passthrough)
- J1 Pin 1 to MP1584EN IN+: Should beep
- MP1584EN OUT+ to ESP32 VIN: Should beep
```

**No Short Circuits:**
```
Test these - should NOT beep:
- 12V+ to GND: OPEN (no beep)
- 5V to GND: OPEN (no beep, except brief if cap charged)
```

### Test 3: Voltage Tests (Power ON - NO ESP32)

⚠️ **Remove ESP32 before this test if socketed!**

1. **Connect 12V power supply to J1**
   - Polarity: Pin 1 = +12V, Pin 2 = GND

2. **Measure voltages:**
   ```
   MP1584EN IN+: Should read ~12V
   MP1584EN OUT+: Adjust until exactly 5.0V ±0.1V
   J2 Pin 1: Should read ~12V
   ```

3. **Adjust MP1584EN:**
   - Use small screwdriver
   - Turn potentiometer slowly
   - Monitor voltage continuously
   - Set to exactly 5.0V

4. **Disconnect power** when done

### Test 4: Full System Test (With ESP32)

1. **Insert ESP32** (if socketed)
2. **Connect 12V power**
3. **Observe ESP32:**
   - Should power on immediately
   - LED should light (red or blue depending on model)
   - Should connect to WiFi (check ESPHome logs)

4. **Check voltages under load:**
   ```
   ESP32 VIN: Should be ~5.0V
   ESP32 3.3V: Should be ~3.3V
   GPIO3: Should be ~0V (no LED connected yet)
   GPIO5: Should be ~0V (default LOW)
   ```

## 🔌 Connection Diagrams

### Power Flow Diagram

```
120V AC Wall
    ↓
[12V DC Adapter]
    ↓ 12V
┌───┴────┐
│ J1 IN  │
└───┬────┘
    ├──────────────────────────┐
    │ 12V                  12V │
┌───▼──────┐            ┌──────▼───┐
│ MP1584EN │            │  J2 OUT  │
│ 12V→5V   │            │   12V    │
└────┬─────┘            └─────┬────┘
     │ 5V                     │
     ├──────┬─────────┐       │
┌────▼───┐  │    ┌────▼───┐   │
│ESP32-C3│  │    │ 470µF  │   │
│  VIN   │  │    │  Cap   │   │
└────────┘  │    └────────┘   │
            │                 │
         [Dehumidifier PCB]◄──┘
            12V, 40W
```

### GPIO Pin Assignments

```
ESP32-C3 Super Mini Pinout:

         ┌──────────┐
    GND  │●        ●│  GND
    GND  │●        ●│  3V3
    GPIO0│●        ●│  GPIO10
    GPIO1│●        ●│  GPIO9
    GPIO2│●        ●│  GPIO8
 ◄─GPIO3 │●        ●│  GPIO7
    GPIO4│●        ●│  GPIO6
 ◄─GPIO5 │●        ●│  RX
    VIN  │●        ●│  TX
         │  USB-C  │
         └────┬────┘
              ↓
          Programming
             Port

GPIO3 ◄── LED Voltage Divider
GPIO5 ──► Touch Control (PN2222 Base)
```

### Signal Flow Diagram

**LED Detection Path:**
```
Dehumidifier LED Anode (2.7V when ON)
    ↓
   J3 Pin 1
    ↓
  10kΩ R1
    ↓
  ┌─┴─┐ ← GPIO3 reads ~1.35V
  │   │
  10kΩ R2
    ↓
   GND
```

**Touch Control Path:**
```
       GPIO5 (3.3V when active)
          ↓
       1kΩ R3
          ↓
    ┌────Base PN2222
    │     │
    │   10kΩ R4 (pull-down)
    │     │
    │    GND
    │
 Collector → J4 Pin 1 → Touch Pad
 Emitter → GND

When GPIO5 HIGH:
- Base gets ~0.7V (through 1kΩ)
- Transistor conducts
- Collector ≈ GND
- Touch pad pulled LOW
- Simulates finger touch
```

## ⚠️ Common Mistakes

### Mistake 1: Incorrect Capacitor Polarity

**Symptom**: Capacitor gets hot, bulges, or explodes
**Fix**: 
- Always identify negative stripe
- Negative goes to GND
- Positive goes to 5V
- If reversed, REPLACE capacitor immediately

### Mistake 2: Wrong PN2222 Orientation

**Symptom**: Touch control doesn't work, transistor gets warm
**Fix**:
- Verify pinout: Flat side facing you = E-B-C (left to right)
- Use multimeter diode mode to test if unsure
- Desolder and reorient if backwards

### Mistake 3: Solder Bridges

**Symptom**: Short circuits, unexpected behavior
**Fix**:
- Inspect under magnification
- Use desoldering braid to remove excess solder
- Apply flux and reheat to improve flow
- Prevent by using less solder and proper iron temperature

### Mistake 4: Cold Solder Joints

**Symptom**: Intermittent connections, bad continuity
**Fix**:
- Joint should be shiny and concave, not dull and blobby
- Reheat joint until solder flows smoothly
- Clean tip and use fresh solder
- Proper technique: Heat pad AND component lead together

### Mistake 5: Forgotten GND Connections

**Symptom**: Components don't work, erratic behavior
**Fix**:
- Verify ALL GND points connected to rail
- Test continuity of GND rail
- Check every component has proper GND return path

### Mistake 6: MP1584EN Not Adjusted

**Symptom**: ESP32 doesn't boot or resets randomly
**Fix**:
- ALWAYS adjust to 5.0V before connecting ESP32
- Measure with multimeter under no load
- Recheck under load (with ESP32 connected)

### Mistake 7: Incorrect Resistor Values

**Symptom**: LED detection doesn't work, touch control erratic
**Fix**:
- Verify resistor color codes with chart
- Measure with multimeter (power OFF)
- Common confusion: 10kΩ vs 1kΩ vs 100Ω
- Replace if wrong value installed

### Mistake 8: GPIO Pin Mix-up

**Symptom**: Swapped functionality or no function
**Fix**:
- Double-check GPIO assignments in code and wiring
- GPIO3 = LED sensing (ADC)
- GPIO5 = Touch control (digital output)
- Some ESP32-C3 boards have different pin layouts

## 📊 Wire Color Convention

Use consistent colors throughout:

| Color | Purpose | Examples |
|-------|---------|----------|
| **Red** | Positive voltage (12V, 5V) | Power supply, VIN |
| **Black** | Ground (GND, 0V) | All GND connections |
| **Green** | Sensor/Input signals | LED detection |
| **Blue** | Control/Output signals | Touch control |
| **Yellow** | 3.3V logic | Optional for logic signals |
| **Orange** | High-side 12V | Optional for 12V signals |

**Benefits:**
- Easier troubleshooting
- Reduces wiring errors
- Professional appearance
- Matches schematic conventions

## 📏 Wire Gauge Guidelines

| Connection | Wire Gauge | Max Length | Notes |
|------------|------------|------------|-------|
| 12V Power (to dehum) | 18-20 AWG | 1m | Carries ~3A |
| 12V Power (to MP1584) | 20-22 AWG | 30cm | Lower current |
| 5V Power | 22-24 AWG | 15cm | <300mA |
| Signal wires | 24-26 AWG | Any | Low current |
| GND returns | 20-22 AWG | Any | Same as power |

**Stranded vs Solid:**
- **Stranded**: More flexible, better for cables that move
- **Solid**: Easier to insert in perfboard holes, stiffer

Use stranded for all JST cables, solid for perfboard connections.

## 🔄 Rework and Repairs

### Removing Solder

1. **Desoldering braid method:**
   - Place braid on joint
   - Heat with iron
   - Solder wicks into braid
   - Cut off used portion

2. **Solder sucker method:**
   - Heat joint until solder melts
   - Quickly place sucker tip
   - Trigger sucker
   - Solder removed by vacuum

### Replacing Components

1. Desolder both leads/pins
2. Gently remove component
3. Clean pads with desoldering braid
4. Install new component
5. Solder in place
6. Retest circuit

### Fixing Broken Traces

1. Scrape away solder mask
2. Tin exposed copper
3. Solder wire bridge across break
4. Secure with hot glue or epoxy
5. Test continuity

## 🧪 Advanced Testing

### Voltage Drop Test

Measure voltage at multiple points under load:
```
MP1584EN OUT: 5.00V
ESP32 VIN: 4.95V (0.05V drop OK)
ESP32 3.3V: 3.30V
```

Large drops indicate:
- Poor connections
- Undersized wire
- Cold solder joints

### Current Draw Test

Normal operation:
- ESP32 idle: ~80-100mA
- ESP32 WiFi TX: ~150-200mA
- Total system: ~200-250mA
- Dehumidifier: ~3000-3500mA (separate)

Excessive current indicates:
- Short circuit
- Faulty component
- Incorrect wiring

### Signal Integrity Test

Use oscilloscope if available:
- GPIO5 square wave: Should be clean 0-3.3V
- GPIO3 ADC: Should be stable DC voltage
- Check for noise/ringing

## 📸 Documentation Tips

**Take photos at each stage:**
1. After component placement (before soldering)
2. After power section complete
3. After signal section complete
4. Top and bottom views of finished board
5. Close-ups of critical connections

**Label your photos:**
- Date and assembly stage
- Any deviations from plan
- Problems encountered and solutions

This helps with:
- Troubleshooting if issues arise
- Building future units
- Contributing improvements to project
- Helping others with similar builds

---

**Next Steps:**
- After completing wiring, proceed to [Testing Guide](TESTING.md)
- Flash ESP32 with firmware from [ESPHome Config](esphome/dehumidifier.yaml)
- Connect to dehumidifier following [Integration Guide](INTEGRATION.md)

**Questions?**
- Check [Troubleshooting Section](README.md#troubleshooting)
- Review photos in `docs/photos/`
- Open issue on GitHub

**⚠️ Remember**: Measure twice, solder once!
