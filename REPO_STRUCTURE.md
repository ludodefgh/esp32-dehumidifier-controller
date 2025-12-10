# GitHub Repository Structure

This document explains the organization of the repository files.

## 📁 Repository Structure

```
esp32-dehumidifier-controller/
│
├── README.md                          # Main project documentation
├── LICENSE                            # MIT License
├── .gitignore                         # Git ignore patterns
├── BOM.md                             # Bill of Materials (parts list)
├── WIRING.md                          # Complete wiring guide
│
├── esphome/                           # ESPHome configuration files
│   ├── dehumidifier.yaml              # Main ESPHome config
│   └── secrets.yaml.template          # Template for WiFi credentials
│
├── docs/                              # Documentation and diagrams
│   ├── dehumidifier_fritzing_diagram.png    # Wiring diagram (Fritzing style)
│   ├── perfboard_layout_visual.png          # Perfboard component layout
│   ├── wiring_guide_copper_side.png         # Copper side wiring guide
│   └── photos/                        # Assembly photos (add your own)
│       ├── assembled-board/
│       ├── dehumidifier-pcb/
│       └── final-installation/
│
└── hardware/                          # Hardware design files (optional)
    ├── schematics/                    # Circuit schematics
    └── 3d-models/                     # 3D printable enclosure (if created)
```

## 📝 File Descriptions

### Root Directory

- **README.md**: Main entry point with project overview, features, installation guide
- **LICENSE**: MIT License for open source distribution
- **.gitignore**: Prevents sensitive files (secrets.yaml) from being committed
- **BOM.md**: Complete list of components with prices and sources
- **WIRING.md**: Detailed step-by-step wiring instructions

### esphome/ Directory

Contains ESPHome configuration files:

- **dehumidifier.yaml**: Complete ESPHome configuration
  - Sensor definitions
  - Control logic
  - Home Assistant integration
  - Well-commented for easy customization

- **secrets.yaml.template**: Template for WiFi credentials
  - Copy to `secrets.yaml` and fill in your details
  - Never commit actual `secrets.yaml` to git

### docs/ Directory

Visual documentation:

- **Fritzing-style wiring diagram**: Complete system overview
- **Perfboard layout**: Component placement guide
- **Copper-side wiring**: Detailed connection instructions

Create subdirectories for your assembly photos:
```bash
mkdir -p docs/photos/{assembled-board,dehumidifier-pcb,final-installation}
```

## 🚀 Getting Started (For Users)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/esp32-dehumidifier-controller.git
cd esp32-dehumidifier-controller
```

### 2. Read Documentation

Start with README.md, then:
1. Review BOM.md and order parts
2. Study WIRING.md while building
3. Reference docs/ diagrams during assembly

### 3. Configure ESPHome

```bash
cd esphome
cp secrets.yaml.template secrets.yaml
# Edit secrets.yaml with your WiFi credentials
```

### 4. Flash ESP32

```bash
esphome run dehumidifier.yaml
```

## 🛠️ For Contributors

### Adding Photos

1. Take clear, well-lit photos during your build
2. Organize in appropriate subdirectories:
   ```
   docs/photos/assembled-board/
   docs/photos/dehumidifier-pcb/
   docs/photos/final-installation/
   ```
3. Use descriptive filenames:
   - `01-components-layout.jpg`
   - `02-power-section-soldered.jpg`
   - `03-esp32-installed.jpg`

### Adding Hardware Files

If you create:
- Circuit schematics (KiCad, Eagle, etc.)
- PCB layouts
- 3D printable enclosures

Add them to:
```
hardware/schematics/
hardware/3d-models/
```

### Documentation Updates

- Keep README.md concise and user-friendly
- Add detailed technical info to separate docs
- Use clear section headers and links
- Include troubleshooting for common issues

## 📋 Checklist for Publishing

Before publishing to GitHub:

- [ ] Remove any personal information from files
- [ ] Verify all links in README.md work
- [ ] Test ESPHome config compiles without errors
- [ ] Add photos to docs/photos/ directory
- [ ] Update BOM.md with current prices
- [ ] Add your name to LICENSE
- [ ] Create GitHub repository
- [ ] Push all files
- [ ] Add topics/tags: esphome, home-assistant, esp32, iot
- [ ] Add description and website URL
- [ ] Consider adding GitHub Actions for YAML validation

## 🏷️ Recommended GitHub Repository Settings

**Topics/Tags:**
- esphome
- home-assistant
- esp32
- esp32-c3
- iot
- smart-home
- diy
- home-automation
- dehumidifier

**Description:**
"Smart dehumidifier controller using ESP32-C3 and ESPHome. Control and monitor via Home Assistant."

**Website:**
Link to your Home Assistant instance or blog post about the project

## 📊 GitHub Features to Enable

1. **Issues**: Enable for bug reports and questions
2. **Discussions**: Enable for general Q&A
3. **Wiki**: Optional for extended documentation
4. **Projects**: Track improvements and enhancements
5. **Releases**: Create releases for stable versions

## 🔒 Security Considerations

**Never commit:**
- `secrets.yaml` (actual WiFi passwords)
- API encryption keys
- Personal network information
- Photos with serial numbers visible
- Home network IP addresses

**The .gitignore is configured to protect:**
- secrets.yaml
- .esphome/ directory (contains device-specific data)
- Personal notes and TODO files

## 📈 Version History

Consider using semantic versioning (semver):

- **v1.0.0**: Initial release
- **v1.1.0**: Added feature X
- **v1.0.1**: Fixed bug Y

Tag releases in git:
```bash
git tag -a v1.0.0 -m "Initial release"
git push origin v1.0.0
```

## 🤝 Contributing Guidelines

If accepting contributions, consider adding:

**CONTRIBUTING.md** with:
- Code style guidelines
- How to submit issues
- Pull request process
- Testing requirements

**CODE_OF_CONDUCT.md**:
- Expected behavior
- Reporting process
- Enforcement

## 📝 Optional Files

Consider adding:

- **CHANGELOG.md**: Document version changes
- **FAQ.md**: Frequently asked questions
- **TROUBLESHOOTING.md**: Common issues and fixes
- **HARDWARE_ALTERNATIVES.md**: Other compatible devices

## 🌐 Promotion

After publishing:

1. Post in Home Assistant Community forums
2. Share in ESPHome Discord
3. Submit to Awesome Home Assistant lists
4. Write blog post with build process
5. Share on Reddit (r/homeassistant, r/esp32)

## 📞 Support

Add links to:
- GitHub Issues for bugs
- GitHub Discussions for questions
- Your preferred contact method
- Home Assistant Community thread

---

**Ready to publish?** Follow this checklist and you'll have a professional, well-organized repository! 🎉
