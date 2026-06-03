# 🚀 Ultra-Smooth Linux Desktop on Android (Termux + Termux-X11)

A lightweight, high-performance guide optimized to run a freeze-free Linux desktop environment inside Termux using **Termux-X11**. 

This configuration is tailored specifically for mid-range arm64 devices (like the Snapdragon 680 / Honor X7d 4G) to eliminate interface lag, stuttering, and random freezing by utilizing a minimal Debian base and disabling heavy window compositing layers.

---

## ✨ Features
* **Debian Slim Footprint:** Minimal idle RAM usage compared to bloated Ubuntu setups.
* **Hardware-Optimized UI:** Pre-configured XFCE4/Openbox sequence with window composition toggled off to prevent GPU-bound UI locks.
* **Automation Automation Script:** Single-command workspace deployment with automated display binding.
* **Stale Session Protection:** Built-in auto-killing mechanism for hung instances or phantom memory leaks.

---

## 🛠 Prerequisites

> [!WARNING]
> **Do not use the Google Play Store versions of Termux.** They are deprecated and will cause repository conflicts or execution hangs.

1. Install the latest stable build of **Termux** from [F-Droid](https://f-droid.org/packages/com.termux/).
2. Install the latest stable release of **Termux-X11** from the official [Termux-X11 Releases](https://github.com/termux/termux-x11/releases).

---

## 🚀 Step-by-Step Installation

### Step 1: Initialize Termux Environment
Open Termux, synchronize repositories, update base packages, and request local storage access:
```bash
pkg update -y && pkg upgrade -y
termux-setup-storage
