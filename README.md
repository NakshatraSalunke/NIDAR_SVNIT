# NIDAR SVNIT Drone Project Documentation

Welcome to the official documentation for our drone project developed for the NIDAR competition by Team SVNIT. This README provides a comprehensive overview of our work, the challenges faced, and our solutions, following the guidelines and requirements from the [NIDAR Official Rulebook v1.1](https://github.com/NakshatraSalunke/NIDAR_SVNIT/blob/main/NIDAR-Official-Rulebook-v1.1.pdf).

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Team Members](#team-members)
3. [Problem Statements](#problem-statements)
4. [System Architecture & Components](#system-architecture--components)
5. [Development Timeline](#development-timeline)
6. [Problems Faced & Solutions](#problems-faced--solutions)
    - [Hardware Challenges](#hardware-challenges)
    - [Software Challenges](#software-challenges)
    - [Integration Challenges](#integration-challenges)
    - [Testing & Field Issues](#testing--field-issues)
7. [Rules and Compliance](#rules-and-compliance)
8. [Lessons Learned](#lessons-learned)
9. [Future Work & Improvements](#future-work--improvements)
10. [References](#references)
11. [Contact](#contact)

---

## Project Overview

This project documents the design, development, and testing of an autonomous drone for the NIDAR competition. The competition features search and rescue tasks in a simulated disaster scenario and precision agriculture, requiring drones to autonomously navigate, detect objects/crops, and relay information back to the ground station within strict safety and technical guidelines.

## Team Members

| Name                | Role                | Responsibilities         |
|---------------------|---------------------|--------------------------|
| [Team Member 1]     | Team Lead           | Project management, compliance, final integration |
| [Team Member 2]     | Hardware Lead       | Frame selection, propulsion, and assembly         |
| [Team Member 3]     | Software Lead       | Navigation, autonomy, and communication           |
| [Team Member 4]     | Payload Specialist  | Camera integration, object detection              |
| [Team Member 5]     | Ground Station Ops  | Data relay, mission control interface             |

---

## Problem Statements

This section outlines the missions (problem statements) as defined by the NIDAR competition:

### Mission 1 - Disaster Management

A coastal town is flooded, affecting the entire area. Water has entered homes, forcing residents to evacuate or take shelter on rooftops without food, water, or medicines. Severe weather initially hindered relief efforts, but after 48 hours, the rain has stopped, and the wind speed has reduced. However, water levels remain high, leaving many people trapped and stranded on their rooftops. National Disaster Response Force (NDRF) and Local Administration teams are ready to start rescue efforts.

**Teams must build two drones to:**
- **Scan an area of 30 hectares** to locate survivors while sending real-time video.
- **Geotag survivors** and communicate to them via a speaker mounted on the drone.
- **Deploy a delivery drone** to deliver survival kits (Size: 5x10x20cm; Weight: 200 gms).

**Additional Details:**
- Scanning and geotagging survivors is considered as Part 1 of the mission, and delivering survival kits is considered as Part 2.
- Each team may build one "Scout Drone" for scanning and locating survivors and another "Delivery Drone" for delivering survival kits.
- Teams may also build both drones as a combination of “Scout+Delivery Drone”. The solution strategy shall remain flexible for teams.
- Both drones must fly autonomously while reporting their status and mission details from a single command and control station.
- Teams also have the option to operate both drones manually with different command and control stations by accepting a penalty (as detailed in the rulebook).

---

### Mission 2 - Precision Agriculture

A village was once a thriving agricultural hotspot, but most residents migrated to cities for modern, high-tech job opportunities. Now, the village struggles with labour shortages during peak crop seasons, and soil quality is deteriorating due to excessive use of pesticides. There is an urgent need to bring pesticide use under control, and residents have decided to adopt modern farming methods and drone technology to implement precision farming.

**Teams must build two drones to:**
- **Scan an area of 2 acres** to identify crops under stress (depicted by pigmentation on leaves), while avoiding obstacles like trees, poles, and wires.
- **Geotag stressed plants** on a map.
- **Deploy a spraying drone** to spray pesticides on the specific plants.

**Additional Details:**
- Scanning and geotagging stressed plants is considered as Part 1 of the mission, and spraying pesticides is considered as Part 2.
- Each team may build one "Scan Drone" to scan and identify stressed crops and another "Spray Drone" to spray pesticides.
- Teams may also build both drones as a combination of “Scan+Spray Drone”. The solution strategy shall remain flexible for teams.
- Both drones must fly autonomously while reporting their status and mission details from a single command and control station.
- Teams also have the option to operate both drones manually with different command and control stations by accepting a penalty (as detailed in the rulebook).

---

## System Architecture & Components

### Hardware

#### Spraying Drone:

- **Frame:** Custom frame, size limited to fit within the 1m x 1m x 0.5m box as per Section 5.2.
- **Motors & ESCs:** Hobbywing X6 Plus 150KV.
- **Propellers:** 2480 propellers. 
- **Battery:** 2x 6S 16000 mAh LiPo batteries in series.
- **Flight Controller:** Cube Orange.
- **GPS Module:** Holybro M9N.
- **Payload:** 
- **Failsafe Mechanisms:** Hardware and software-based, including manual override (rule 5.3.13).

### Software

- **Autonomous Navigation:** Implemented using mission planner and onboard software.
- **Object Detection:** ML-based color/shape detection for target identification (rule 6.4.1).
- **Communication:** 2.4GHz telemetry for control, 5.8GHz for video relay, both within allowed frequencies.
- **Failsafes:** Return-to-home and emergency land protocols.

### Block Diagram

```
[Ground Station] <--Telemetry--> [Flight Controller] <--ESCs/Motors/Servos-->
                               |                      |
                               |                      +---[Camera/Video Relay]
                               |
                               +---[GPS/IMU/Sensors]
```

---

## Development Timeline

| Phase         | Description                          | Duration      |
|---------------|--------------------------------------|--------------|
| Planning      | Rulebook analysis, team formation    | Mar 2025     |
| Prototyping   | Frame assembly, initial component tests | Apr 2025  |
| Integration   | Onboard software and payload integration | May 2025  |
| Testing       | Indoor and outdoor flight tests, mission simulation | May-Jun 2025 |
| Finalization  | Compliance check, documentation, and final demo | Jun 2025 |

---

## Problems Faced & Solutions

This section is the core of our documentation, outlining the main challenges encountered and the solutions or workarounds devised.

### Hardware Challenges

#### Problem 1: Weight and Size Compliance
- **Description:** The frame and payload initially exceeded the 3kg limit and 1m x 1m footprint (rule 5.2).
- **Solution/Workaround:** Switched to lighter frame materials, re-evaluated payload, and used 3D-printed mounts.
- **Outcome:** Achieved a final takeoff weight of 2.7kg and fit all components within the specified dimensions.

#### Problem 2: Propeller Safety
- **Description:** Propeller tips were initially exposed, violating rule 5.3.7.
- **Solution/Workaround:** Added custom propeller guards and designated a safe arming area.
- **Outcome:** Passed pre-flight safety checks.

### Software Challenges

#### Problem 1: GPS Signal Multipath Errors
- **Description:** GPS position drifted in test environments, causing mission deviations.
- **Solution/Workaround:** Implemented sensor fusion with IMU and barometer data; set software geofence for failsafe triggers (rule 5.3.12).
- **Outcome:** Improved navigation accuracy and ensured the drone did not exit the field.

#### Problem 2: Object Detection Under Varying Lighting
- **Description:** Detection algorithm failed under low light and glare.
- **Solution/Workaround:** Added adaptive thresholding and trained detection model with images under diverse lighting.
- **Outcome:** Detection accuracy improved to >90% in all field conditions.

### Integration Challenges

#### Problem 1: Video Transmission Reliability
- **Description:** 5.8GHz video stream suffered from interference, causing lost frames.
- **Solution/Workaround:** Tried different antennas, improved shielding, and optimized bitrate to stay within allowed frequencies and power (rule 5.3.10).
- **Outcome:** Reduced video dropouts to less than 5% of frames.

#### Problem 2: Failsafe Activation During Mission
- **Description:** Spurious GPS glitches triggered failsafe land mid-mission.
- **Solution/Workaround:** Adjusted failsafe thresholds, and enabled manual override (rule 5.3.13).
- **Outcome:** No unintended failsafes during later tests.

### Testing & Field Issues

#### Problem 1: Wind Gust Instability
- **Description:** Drone lost stability in gusty outdoor conditions.
- **Solution/Workaround:** Tuned PID controllers, added wind compensation in software.
- **Outcome:** Maintained stable flight in up to 15km/h winds.

#### Problem 2: Battery Endurance
- **Description:** Battery voltage dropped rapidly under full payload.
- **Solution/Workaround:** Switched to higher capacity cells, balanced payload, and optimized flight path for efficiency.
- **Outcome:** Achieved consistent 12-minute endurance under mission load.

---

## Rules and Compliance

- **Safety:** All flights conducted with safety as top priority and in compliance with Section 5.3.
- **Frequency Usage:** Only 2.4GHz and 5.8GHz bands used for telemetry and video, with output power under allowed limits.
- **Failsafes:** Manual override and return-to-home implemented as required.
- **Autonomy:** Drone operates fully autonomously after takeoff, in accordance with Section 6 mission requirements.
- **Mission Objectives:** Search, detection, geotagging, and signaling are fully automated; results are relayed to the ground station as specified.

---

## Lessons Learned

- Early review of the rulebook is essential for compliance and avoiding rework.
- Weight management must be prioritized in both hardware and software design.
- Real-world testing uncovers integration issues that simulations do not.
- Robust failsafes and manual overrides are vital for both safety and mission success.
- Lighting conditions can drastically affect object detection — prepare for all scenarios.

---

## Future Work & Improvements

- Integrate multi-modal sensors (thermal, LIDAR) for improved detection in adverse conditions.
- Explore alternative power sources or hybrid propulsion for greater endurance.
- Implement advanced swarm behaviors for multi-drone missions.
- Develop custom ground station UI for better operator situational awareness.
- Add redundancy in communication systems to further reduce risk of data loss.

---

## References

- [NIDAR Official Rulebook v1.1 (PDF)](https://github.com/NakshatraSalunke/NIDAR_SVNIT/blob/main/NIDAR-Official-Rulebook-v1.1.pdf)
- [Pixhawk Documentation](https://docs.px4.io/)
- [Mission Planner Software](https://ardupilot.org/planner/)
- [Dronecode SDK](https://sdk.dronecode.org/)
- [OpenCV for Object Detection](https://opencv.org/)
- [Relevant datasheets and component manuals]

---

## Contact

For questions or collaborations, please contact:
- [Team Leader Name] ([email])
- [Other relevant contacts]

---

*This documentation is intended to be a living document. Please update any new problems, solutions, and insights as the project evolves!*
