# Rheinwerk – "Controlled Chaos" Vehicle Design System

[![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-lightgrey)](LICENSE)
[![CAD Status](https://img.shields.io/badge/CAD-Release%20Candidate-brightgreen)](https://github.com/rheinwerk/01-cad)
[![UI Build](https://img.shields.io/badge/UI%20Core-v2.3.1-blue)](https://github.com/rheinwerk/ui-core)
[![Discord](https://img.shields.io/badge/Community-Join-7289da)](https://discord.gg/rheinwerk)

> *"We were bored of the single piece of soap."* – Elara Voss, CDO

**Rheinwerk Automotive** is an open-design initiative to reforge German automotive DNA. We are building the *01* — an electric performance coupe that prioritizes tactility, parametric surfaces, and emotional feedback over touchscreen ubiquity.

<img width="388" height="234" alt="image" src="https://github.com/user-attachments/assets/71690871-d07c-45f0-bac0-691aeb74e2ae" />


##  Philosophy

Legacy automakers are adding 50kg of sound deadening and 1,000 lines of code to hide the fact that cars are getting heavier. **Rheinwerk is about visceral reduction.** We want the car to feel like a mechanical watch that happens to be electric.

- **No giant touchscreens** – One physical "Command Core" controls everything.
- **Parametric exteriors** – Microscopic dimples replace static character lines.
- **Open source design** – CAD files for the dashboard and core UI logic are freely available for non-commercial use.

---

##  Repository Structure

```
rheinwerk-01/
├── cad/
│   ├── exterior/
│   │   ├── parametric_dimples.stp      # Surface dimple array logic
│   │   ├── beltline_parabolic_arc.stp
│   │   └── active_aero_flaps.stp
│   ├── interior/
│   │   ├── command_core_knurled.stp    # The aluminum controller
│   │   └── seat_scoring_surface.stp
│   └── assembly/
│       └── full_vehicle_v2.3.stp
├── firmware/
│   ├── haptics/
│   │   └── command_core_feedback.ino   # Tactile motor control
│   ├── sound/
│   │   ├── v10_sample.wav              # Raw F1 V10 recording
│   │   ├── berlin_synth_profile.fft    # DJ-processed harmonic map
│   │   └── driver_character/           # Modes: Angry/Calm/Techno
│   └── steering/
│       └── feedback_curves.json        # Torque mapping per mode
├── ui/
│   ├── passenger_screen/               # React-based infotainment
│   └── command_core_animations/
├── docs/
│   ├── philosophy.md                   # Full design manifesto
│   ├── legal_dimples.md                # Regulatory workarounds (EU)
│   └── crash_test_simulation.pdf
└── README.md
```

---

##  Getting Started

### For CAD Designers (Fusion 360 / SolidWorks)

```bash
git clone https://github.com/rheinwerk/rheinwerk-01.git
cd rheinwerk-01/cad/exterior
# Open .stp files in your preferred STEP viewer
```

**Recommended:** Use the *parametric_dimples.stp* as a template for generative surface texturing.

### For Embedded Engineers (Arduino / PlatformIO)

Flash the haptic feedback firmware to a test unit:

```bash
cd firmware/haptics
platformio run --target upload
```

### For Sound Designers

Load `v10_sample.wav` into your DAW, then apply the `berlin_synth_profile.fft` as an FFT convolution filter.

---

##  The Command Core – API Reference

The physical aluminum dial communicates over CAN bus (simulated via USB-C in dev kits). It reports three values:

| Mode        | Command Value | Haptic Response          |
|-------------|---------------|--------------------------|
| "Angry"     | `0x01`        | 60Hz square wave, sharp  |
| "Calm"      | `0x02`        | 30Hz sine wave, smooth    |
| "Techno"    | `0x03`        | Syncopated pulses @ BPM   |

**Example Python listener:**
```python
import can
bus = can.interface.Bus(channel='can0', bustype='socketcan')
for msg in bus:
    if msg.arbitration_id == 0x201:  # Command Core
        mode = msg.data[0]
        print(f"Switching to mode: {['Angry','Calm','Techno'][mode-1]}")
```

---

## 🧪 Build Status

| Component               | Status      | Next Milestone                     |
|-------------------------|-------------|------------------------------------|
| Parametric Exterior     |  RC1        | Legal dimple certification (Q3)    |
| Command Core Haptics    |  Prototype  | Production tooling (Q4)            |
| V10 + Techno Sound Mix  |  Beta       | Final DJ mastering (Aug)           |
| Active Aero Flaps       |  Draft      | Wind tunnel validation needed      |
| EU Crash Homologation   |  Planned    | Q1 2027                            |

---

##  Roadmap

- **2026 Q4** – Limited prototype drive events (Nürburgring)
- **2027 Q1** – Start of production for 999 coupes
- **2027 Q3** – Open-source *Stromer* sedan design files released
- **2028** – Community-driven "Hack the Core" firmware competition

---

##  Contributing

We welcome contributions that respect the *visceral reduction* philosophy.

**Areas of need:**
- Aerospace engineers to validate active aero flap legality
- Embedded Rust devs for low-latency haptic drivers
- UI designers for the passenger screen (React + WebGL)
- Ethical hackers to penetration-test the CAN bus security

**Contribution process:**
1. Read `docs/philosophy.md`
2. Fork the repo
3. Submit a PR with CAD screenshots or firmware benchmarks
4. Join our Discord for design reviews

---

##  License

- **CAD files & design assets:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) (free for non-commercial, attribution required)
- **Firmware & UI code:** MIT License (open for any use)
- **Sound samples:** Proprietary – licensed for personal, non-commercial use only

---

##  Community

- [Discord Server](https://discord.gg/rheinwerk) – #cad-critique, #haptic-dev, #techno-sound
- [Twitter/X](https://twitter.com/rheinwerk) – Daily renders and sarcasm
- Physical meetups: Second Saturday of every month, Munich (former Bavarian foundry)
- <img width="678" height="959" alt="image" src="https://github.com/user-attachments/assets/8939f4d8-60bc-40d6-8172-60815f3bf5b8" />


---

##  Acknowledgments

- Defectors from Porsche Sonderwunsch, BMW M, and IDEO Munich
- Berlin techno DJ collective *Frictio*
- The open-source automotive community at OScar
