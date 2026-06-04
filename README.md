# Intelligent Traffic Signal Monitoring Using Deep Learning

**Author:** Nihil VS | **USN:** 23BTRCA038  
**Institution:** FET – Jain Deemed to Be University, Bengaluru

---

## Overview

A real-time adaptive traffic signal control system that:
- Detects and counts vehicles from CCTV footage using **YOLOv8n**
- Tracks vehicles across frames using **ByteTrack**
- Dynamically adjusts green signal duration based on traffic density
- Automatically detects **ambulances** and triggers a **50-second GREEN override**

---

## Demo Output

| Frame 54 — RED, LOW Density | Frame 196 — Ambulance Priority | Frame 418 — Post-Clearance |
|---|---|---|
| 1 vehicle, avg 2.8, 10s green | Immediate GREEN override, 50s | GREEN still active, 38.5s left |

---

## How It Works

### Traffic Signal State Machine
The signal cycles through RED → GREEN → YELLOW → RED, with green duration
determined by real-time vehicle density:

| Density | Vehicle Count | Green Time |
|---------|--------------|------------|
| LOW     | < 5          | 10 seconds |
| MEDIUM  | 5–10         | 22 seconds |
| HIGH    | > 10         | 38 seconds |
| AMBULANCE OVERRIDE | Any | 50 seconds |

### Ambulance Detection (3-Layer Heuristic)
1. **Layer 1 – Model Label:** If YOLOv8n directly classifies the vehicle as `ambulance`
2. **Layer 2 – White Pixel Ratio:** Checks HSV color space for white/cream regions (>40% of ROI)
3. **Layer 3 – Siren Light Detection:** Detects red/blue siren pixels in the ROI

---

## Performance Summary

| Metric | Outcome |
|--------|---------|
| Detection Speed | ~15–16 FPS (CPU) |
| Low Density Green | 10s (vs. 30s baseline) |
| Medium Density Green | 22s |
| High Density Green | 38s |
| Ambulance Override | 50s (triggered at Frame 196) |
| Session Vehicle Count | 32 unique vehicles |
| Green Time Reduction | ~60% at low density |

---

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/intelligent-traffic-signal-monitoring
cd intelligent-traffic-signal-monitoring
pip install -r requirements.txt
```

---

## Usage

1. Place your traffic video file in the project folder and rename it `videoplayback.mp4`
2. Run the script:

```bash
python Python.py
```

### Keyboard Controls
| Key | Action |
|-----|--------|
| `ESC` | Quit |
| `A` | Manually trigger ambulance override |
| `P` | Pause / Resume |
| `S` | Save screenshot |

---

## Project Structure
