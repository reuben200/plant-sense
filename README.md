# Project Definition Report (PDR)
## Real-Time Hand Gesture Recognition for Audio Playback Control (Edge AI)

---

## 1. Project Title
**Real-Time Hand Gesture Recognition for Audio Playback Control (Edge AI)**

---

## 2. Problem Statement
Controlling media playback typically requires physical interaction through keyboard, mouse, or touchscreen devices, which is not always convenient or hands-free. This project solves this problem by using computer vision to recognize simple hand gestures in real time through a webcam and trigger audio playback actions — **offline and on-device**, without cloud processing.

---

## 3. Objectives
- Implement real-time **hand detection and gesture recognition** on a local machine.
- Run all inference **on-device** (Edge AI, offline).
- Map gestures to actions:
  - **Open palm → Play audio**
  - **Fist → Stop audio**
- Demonstrate gesture-driven human–computer interaction.
- Provide a foundation for future gesture-based controls (volume, next song, etc).

---

## 4. Scope
### In Scope
- Webcam-based hand tracking (MediaPipe Hands).
- Intermediate-level gesture detection (open vs fist).
- Offline playback of a built-in tone/audio.
- Execution using Python on a laptop (no external device).

### Out of Scope (for Version 1)
- System-wide media control (Spotify, VLC, OS-level API).
- Multi-gesture actions (next track, volume, etc).
- Mobile deployment (can be future version).
- Custom model training (MediaPipe pre-trained only).

---

## 5. Tools & Technologies
| Category | Technology |
|---------|-------------|
| Inference | MediaPipe |
| Vision Input | Laptop webcam |
| Programming Language | Python |
| Audio Playback | Built-in tone / Python audio library |
| OS Requirement | Windows, macOS, or Linux |
| Deployment | Local offline execution |

---

## 6. System Overview
The webcam captures live video frames. MediaPipe processes each frame locally and identifies key hand landmarks. Based on the landmark arrangement, the system classifies the hand gesture (open palm or fist). The recognized gesture triggers an action — start or stop playing an audio tone — in real time.

---

## 7. Architecture Diagram (Conceptual)

```
Webcam → MediaPipe → Gesture Logic → Action Trigger → Audio Playback
```

---

## 8. Data Flow

| Step | Process |
|------|---------|
| 1 | Webcam captures frame |
| 2 | Frame sent to MediaPipe hand tracking |
| 3 | Landmarks extracted & interpreted |
| 4 | Gesture (open / fist) recognized |
| 5 | If open → Play audio |
| 6 | If fist → Stop audio |

---

## 9. Success Criteria
- System detects a hand gesture in real time.
- Open palm → reliably starts the audio.
- Fist → reliably stops the audio.
- All processing happens **locally and offline**.
- Works with standard laptop webcam.

---

## 10. Future Extensions
- Add more gesture actions (thumbs up/down, V-sign, etc).
- Control external media players (Spotify / VLC).
- Slide presentation control for teaching/speaking.
- Deployment on mobile or Raspberry Pi.
- Sign language interpretation (advanced).
- Custom gesture classifier training.

---

## 11. Version
This document describes **Version 1** of the project.
