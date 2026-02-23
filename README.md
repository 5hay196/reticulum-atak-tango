# Reticulum + ATAK + Project Tango

Reticulum mesh networking integration with ATAK (Android Team Awareness Kit) and Project Tango devices. This repository explores hardware compatibility, CoT bridging, and 3D sensor fusion for off-grid situational awareness.

## 🚀 Overview

Integrating the **Reticulum Network Stack (RNS)** with **ATAK** on legacy **Project Tango** hardware provides a unique platform for:
- Indoor 3D mapping and SLAM.
- Encrypted, infrastructure-less mesh communication.
- Geospatial situational awareness in denied environments.

---

## 🛠 Technical Requirements

### Reticulum Network Stack (RNS)
- **Runtime:** Python 3.x (runs entirely in userland).
- **Interface Support:** LoRa (via RNode), Serial, WiFi/Ethernet (AutoInterface), I2P, and Packet Radio.
- **Bandwidth:** Operates on links as low as 500 bps.
- **Security:** Default end-to-end encryption (OpenSSL/PyCA or pure-Python fallbacks).

### ATAK-CIV baseline
- **Android version:** 5.0 (API 21) or later.
- **GPU:** GLES 3.0 support required.
- **Recommended:** Samsung S9-class hardware or better for modern builds.

### Project Tango Hardware (Yellowstone Tablet / Phone Dev Kits)
- **Yellowstone Tablet:** Tegra K1, 4GB RAM, 128GB Storage, Android 4.4 KitKat (Base).
- **Challenge:** Most Tango devices ship with Android 4.4, which is below the ATAK-CIV 5.0 minimum.

---

## 🗺 Implementation Paths

### 1. The Research Platform (Plugin Architecture)
Treat ATAK as the UI layer and build a custom plugin to bridge Tango sensors:
- **Tango SDK:** Access motion tracking, depth sensing, and area learning.
- **CoT Bridging:** Convert depth/pose data into **Cursor-on-Target (CoT)** messages.
- **Reticulum Transport:** Use RNS to broadcast CoT data across the mesh.

### 2. The Auxiliary Sensor Node
Keep Tango on its native stack (Android 4.4) and use it as a dedicated 3D mapper:
- Generate point clouds or SLAM maps on the Tango device.
- Export as KML/KMZ or tile overlays.
- Sideload products to a modern primary ATAK host (Android 5.0+).

### 3. Custom ROM / OS Upgrade
Attempt to flash a community-supported Android 5.0+ ROM to meet ATAK requirements directly. This is high-risk but enables running ATAK-CIV as a native app on the Tango hardware.

---

## 🔗 Resources & Integration

### CoT (Cursor on Target) Integration
- **atak-push-cots:** Python library for pushing CoT markers via network interfaces.
- **Reticulum-Telemetry-Hub:** Bridge for sharing telemetry across RNS nodes.
- **takproto:** Python module for encoding/decoding TAK-specific CoT messages.

### Installation (Termux / Android)
To run Reticulum on Android via Termux:
```bash
pkg update && pkg upgrade
pkg install python python-cryptography
pip install rns
```

---

## ⚖ Disclaimer
Project Tango support officially ended in 2017. Hardware should be treated as a development/experimental platform. Native Tango integration is not available \"out of the box\" and requires custom plugin development.
