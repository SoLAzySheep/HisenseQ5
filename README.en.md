# Hisense Q5 Screen Calibration

[Simplified Chinese](README.zh-CN.md) · [English](README.en.md)

A Root-based display calibration tool for the Hisense Q5 RLCD tablet. Current version: `0.1.51`.

## Features

- Adjust panel contrast, panel saturation, Gamma, sharpness, and red/green/blue gains.
- Adjust Android system saturation separately from panel HSV saturation; the two stages can combine visually.
- Use synchronized sliders and values in the main screen and floating panel. Dragging applies changes; tapping a value opens absolute-value input.
- The floating panel opens expanded from the main screen and can be moved, collapsed, reopened, or closed.
- The heart button applies a fixed recommended preset. Ordinary tuning does not change the preset.
- Restore disables panel modules enabled by this app and restores its saved system-saturation baseline. It does not erase device data.

## Device and permissions

| Item | Verified target |
|---|---|
| Device | Hisense Q5 HITV105C / T610 |
| Minimum Android | Android 8.0 (API 26); tested on Android 10 |
| Firmware | `L1721.6.01.02` |
| Panel driver | `dpu-r4p0` |
| Root | Required; grant access through Magisk |
| Floating window | Requires the Android “Display over other apps” permission |

Before panel writes, the app checks the device fingerprint and DPU version. Only the Q5 firmware listed above has been verified. Other devices, firmware, and DPU versions are not guaranteed to work. There is no Shizuku tuning backend.

Accepted fingerprint for panel writes: `Hisense/HITV105C/HITV105C:10/QP1A.190711.020/L1721.6.01.02:user/release-keys`.

## Tuning ranges

| Parameter | Range |
|---|---:|
| Contrast | 0.00–31.99 |
| Panel saturation | 0.00–64.00 |
| Gamma | 0.10–113.04 |
| Sharpness | 0.00–1.60 |
| Red, green, and blue gains | 0.00–31.99 each |
| System saturation | 0.00–2.00 |

Panel sliders use a nonlinear mapping with neutral `1.00` at the midpoint. Absolute-value input uses `0.01` precision. Limits are derived from the bundled vendor table and DPU field encodings; they are encoding boundaries, not recommended viewing values. Excessive settings can distort colors, clip highlights, or remove detail.

## Install and use

1. Install the APK on a verified Q5.
2. Allow Root access in the Magisk prompt.
3. Grant the floating-window permission in Android settings before using the overlay.
4. Drag a slider to tune, or tap its value to enter an absolute value. `√` saves and applies; `×` cancels.
5. Tap the heart to apply the fixed recommended preset. Ordinary slider changes do not modify it.

## Device implementation

Panel controls write CM, HSV, Gamma, and EPF data through the Q5 `/sys/class/display/dispc0/PQ/` interface and check module status. Payload lengths verified on the target device are 24 bytes for CM, 1440 for HSV, 1536 for Gamma, and 14 for EPF.

The app builds tuning data from the bundled vendor XML and does not read the device's binary Gamma table; reading that table on this firmware triggers a kernel `dpu_enhance_get` error. System saturation uses SurfaceFlinger's transaction `1022` through Root and remains separate from panel DPU controls. MediaTek PQ interfaces from Paper7 and TCL S8 are not reused on the Q5.

## Build from source

Requirements: Windows PowerShell, JDK 17, Android SDK Platform 35, Build Tools 35.0.0, and `~/.android/debug.keystore`. The vendor tuning XML files are included under `assets/pq/`.

```powershell
.\build.ps1 -Sdk "C:\Android\Sdk" -Jdk "C:\Java\jdk-17"
```

The script builds and signs `build/Q5-screen-tuner-0.1.51.apk`. To install on a connected device:

```powershell
adb install -r .\build\Q5-screen-tuner-0.1.51.apk
```
