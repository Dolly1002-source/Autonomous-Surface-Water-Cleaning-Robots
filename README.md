# Autonomous Surface Water Cleaning Robot
📄 **Paper:** [Autonomous Surface Water Cleaning Robots (IJSREM, 2024)](https://doi.org/10.55041/IJSREM32535)

A prototype unmanned surface vehicle (USV) designed to collect floating waste from lakes, ponds, and rivers. It combines ultrasonic obstacle avoidance (Arduino Uno) with camera-based waste classification (Raspberry Pi 4 and TensorFlow Lite).

This project is described in the published paper:

> **Autonomous Surface Water Cleaning Robots**
> Reshika N and Dr. Vasudeva G
> *International Journal of Scientific Research in Engineering and Management (IJSREM)*, Volume 08, Issue 05, May 2024
> DOI: [10.55041/IJSREM32535](https://doi.org/10.55041/IJSREM32535)

---

## Repository Status

This repository documents the design of the project. The original source code and hardware prototype are **not included**. If reference code is added later, it will be clearly labeled as a re-implementation based on the paper, not the original prototype code.

---

## Problem

Urban lakes and ponds collect plastic, biomedical waste, and other debris. Manual cleaning is slow, labor-intensive, and exposes workers to infectious microorganisms. Boats and chemical treatments are costly and can harm the ecosystem. This project proposes an automated, low-cost, battery-powered alternative.

## System Overview

| Subsystem | Role |
|---|---|
| **Arduino Uno** | Controls locomotion and obstacle avoidance |
| **Raspberry Pi 4 + Pi Camera** | Image processing and waste classification |
| **GPS module** | Navigation to specified locations |
| **Ultrasonic sensor (HC-SR04)** | Detects obstacles and water-body boundaries |
| **Servo motor (SG90)** | Rotates the sensor to scan left and right |
| **DC motors + propellers** | Propulsion (47 mm propellers) |
| **Rechargeable batteries** | Power source |

### Mechanical Design

- Two **EPS foam pontoons** provide buoyancy and stability.
- A **salvage net** between the pontoons collects debris, supported by an **acrylic frame**.
- Motors are embedded in each pontoon and connected to propellers through shafts designed to prevent water ingress.
- A **sealed, watertight enclosure** above the frame protects the electronics.

## Obstacle Avoidance Logic

The Arduino uses an ultrasonic sensor mounted on a servo:

1. Move forward and continuously measure distance.
2. If an obstacle is detected within the set range, stop and scan right and left with the servo.
3. Turn toward the side with more clearance. If both sides are equal, turn left.
4. Pause briefly to realign after turning.
5. If several consecutive obstacles are detected (flag count reaches 3), enter **recovery mode** and reverse for a set duration.
6. If no obstacle is detected, resume moving forward.

## Waste Classification

- **Tool:** Google Teachable Machine (transfer learning on MobileNet)
- **Classes:** trash, plastic, paper, leaf
- **Dataset:** images from Kaggle plus extra photos taken with a laptop webcam

| Class | Images |
|---|---|
| Trash | 137 |
| Plastic | 482 |
| Paper | 594 |
| Leaf | 311 |

- **Deployment:** the trained model was exported as `model.tflite` (TensorFlow Lite floating-point), copied to the Raspberry Pi 4 using VNC Viewer, and run on-device with the TensorFlow Lite runtime.
- Classified waste is stored in an onboard container for later disposal or recycling.

> The paper does not report quantitative accuracy results for the classifier.

## Advantages

- Reduces manual labor and worker exposure to hazardous water
- Battery-powered, with no fossil fuels
- Low-cost components
- Designed to operate with minimal disturbance to aquatic life

## Future Scope

- Field testing in more challenging environments
- More robust obstacle avoidance and cleaning methodology
- Additional sensors and advanced algorithms for better detection and efficiency
- Water-quality monitoring (pH, turbidity, dissolved oxygen)

## Tech Stack

Arduino Uno, Raspberry Pi 4, Python, TensorFlow Lite, Google Teachable Machine, HC-SR04, SG90, GPS module

## Citation

```
Reshika N and Vasudeva G, "Autonomous Surface Water Cleaning Robots,"
International Journal of Scientific Research in Engineering and Management (IJSREM),
vol. 8, no. 5, May 2024. DOI: 10.55041/IJSREM32535
```









