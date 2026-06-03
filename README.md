# 🚀 Ultra-Smooth Linux Desktop on Android (Termux + Termux-X11)

A fully automated, zero-config setup optimized to run a freeze-free Linux desktop environment inside Termux using **Termux-X11**. 

This repository provides an all-in-one automation workflow engineered explicitly for mid-range arm64 devices (such as the Snapdragon 680 / Honor X7d 4G) running modern Android versions. It addresses performance bottlenecks by deploying a slimmed-down Debian base, turning off hardware window compositing layers that stall the GPU, and wrapping the initialization routine into a single executive script.

---

## ✨ Key Features
* **Zero-Touch Setup:** One master script handles file system preparation, package installations, configurations, and environment variables.
* **Debian Slim Framework:** Exceptional performance with minimal background resource footprint compared to generic Ubuntu deployments.
* **Anti-Freeze Optimization:** Hard-coded configuration rules automatically disable window compositing (`xfwm4`) to eliminate UI locks and video driver hangs.
* **Stale Session Sanitation:** Automation routines auto-purge hung processes (`Xwayland`, `virglrenderer`, etc.) before spawning new server sockets.

---

## 🛠 Prerequisites

> [!WARNING]
> **Do not use the Google Play Store version of Termux.** It has been deprecated for years and will cause compilation errors, frozen repository locks, or package version conflicts.

1. Download and install the latest stable APK framework of **Termux** via [F-Droid](https://f-droid.org/packages/com.termux/)[cite: 1].
2. Download and install the latest companion architecture APK of **Termux-X11** directly from the official [Termux-X11 Releases](https://github.com/termux/termux-x11/releases) (Download the universal artifact or matching arm64-v8a build).

---

## 🚀 One-Command Master Installation

Open your freshly installed **Termux terminal app** and paste the entire unified command block below into the command line, then press **Enter**:

```bash
# ====================================================================
# MASTER AUTOMATED FAST & SMOOTH DEBIAN SETUP FOR TERMUX + TERMUX-X11
# Optimized for Snapdragon 680 / Honor X7d 4G (Compositing Disabled)
# ====================================================================

echo "[*] Synchronizing, updating core repositories and upgrading baseline packages..."
pkg update -y && pkg upgrade -y

echo "[*] Granting local file system storage permissions..."
termux-setup-storage

echo "[*] Injecting specialized repository mirrors..."
pkg install x11-repo tur-repo -y

echo "[*] Fetching virtualization layers, display servers, and text utility packages..."
pkg install termux-x11-nightly proot-distro nano ncurses-utils -y

echo "[*] Fetching and installing the ultra-lightweight Debian base..."
proot-distro install debian

# --------------------------------------------------------------------
# STEP A: INJECTING INNER DESKTOP RUNNER DIRECTLY FROM HOST ENVIRONMENT
# --------------------------------------------------------------------
echo "[*] Generating container runner loop parameters inside Debian..."

# Safely mount inner runner configuration paths out-of-box using raw text streams
cat << 'EOF' > /data/data/com.termux/files/usr/var/lib/proot-distro/installed-distros/debian/usr/local/bin/termux-desktop-runner
#!/bin/bash
export DISPLAY=:1
export PULSE_SERVER=127.0.0.1
export XDG_CURRENT_DESKTOP=XFCE

# CRITICAL OPTIMIZATION: Disables hardware compositing engine to stop rendering freezes
xfconf-query -c xfwm4 -p /general/use_compositing -s false

# Start dbus message session cleanly alongside the user interface framework
dbus-launch --exit-with-session startxfce4
EOF

# Grant absolute execution rights to the newly created internally hosted script
chmod +x /data/data/com.termux/files/usr/var/lib/proot-distro/installed-distros/debian/usr/local/bin/termux-desktop-runner

# --------------------------------------------------------------------
# STEP B: INJECTING WORKSPACE PACKAGES INTO DEBIAN ISOLATION LAYER
# --------------------------------------------------------------------
echo "[*] Entering container runtime to initialize and load GUI packages (This may take a few minutes)..."
proot-distro login debian --shared-tmp -- sh -c "
  apt update && apt upgrade -y;
  apt install xfce4 xfce4-terminal desktop-base lightdm dbus-x11 shared-mime-info tango-icon-theme -y;
"

# --------------------------------------------------------------------
# STEP C: CONFIGURING THE HOST AUTOMATION LAUNCHER SCRIPT
# --------------------------------------------------------------------
echo "[*] Compiling the executive master launch script inside Termux home space..."

cat << 'EOF' > ~/start-desktop.sh
#!/data/data/com.termux/files/usr/bin/bash

# Purge any stale, frozen, or broken background server loops automatically
echo "[!] Clearing hung display instances..."
killall -9 termux-x11 Xwayland virglrenderer 2>/dev/null

echo "[*] Initializing high-speed Termux-X11 connection port..."
termux-x11 :1 -xstartup "proot-distro login debian --shared-tmp -- termux-desktop-runner" &

# Allow brief sleep window for background processes to map variables
sleep 2.5

echo "[*] Forcing visual projection interface layer to the foreground..."
am start -n com.termux.x11/com.termux.x11.MainActivity
EOF

# Grant active execution authorization privileges across the final script
chmod +x ~/start-desktop.sh

clear
echo "================================================================"
echo " 🎉 INITIAL SETUP COMPLETED PERFECTLY!"
echo "================================================================"
echo " 🛠️ HOW TO DEPLOY YOUR NEW SMOOTH WORKSPACE:"
echo " 1. Make sure you have open the Android Termux-X11 App once."
echo " 2. Type or paste the following deployment handle right here:"
echo "    ./start-desktop.sh"
echo "================================================================"
