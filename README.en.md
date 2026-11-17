# Hisense Q5 Screen Calibration

[Simplified Chinese](README.zh-CN.md) · [English](README.en.md)

A Root-based display calibration tool for the Hisense Q5 RLCD tablet.

## Features

- Adjust panel contrast, panel saturation, Gamma, sharpness, and red/green/blue gains.
- Adjust Android system saturation separately from panel HSV saturation; the two stages can combine visually.
- Use synchronized sliders and values in the main screen and floating panel. Dragging applies changes; tapping a value opens absolute-value input.
- The floating panel opens expanded from the main screen and can be moved, collapsed, reopened, or closed.
- The heart button applies a fixed recommended preset. Ordinary tuning does not change the preset.
- Restore disables panel modules enabled by this app and restores its saved system-saturation baseline. It does not erase device data.

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

