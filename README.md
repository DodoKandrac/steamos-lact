<!--Author: D.A.Pelasgus-->
<p align="center"><img src="assets/logo.svg" alt="ChimeraOS" style="width: 150px;" /></p>

[![status](https://img.shields.io/badge/status-stable-%23961937.svg?style=for-the-badge)](https://github.com/chimeraos/install-media/releases/latest)
[![License](https://img.shields.io/badge/License-MIT-%23961937.svg?style=for-the-badge)](https://github.com/ChimeraOS/chimeraos/blob/master/LICENSE)
[![Chat Server](https://img.shields.io/badge/chat-discord-%23961937.svg?style=for-the-badge)](https://discord.gg/fKsUbrt)
[![website](https://img.shields.io/badge/website-chimeraos.org-%23961937.svg?style=for-the-badge)](https://chimeraos.org)
[![Made with Love](https://img.shields.io/badge/made_with-❤-%23961937.svg?style=for-the-badge)](https://chimeraos.org)

Bringing the console experience to PC.

> [!CAUTION]
> DO NOT DOWNLOAD DIRECTLY FROM THE RELEASES PAGE.
> THIS IS NOT INSTALLATION MEDIA.

> [!IMPORTANT]
> To download use the following link:
> [ChimeraOS website](https://chimeraos.org)

> [!WARNING]
> Steam Deck is not supported by this fork. An AMD GPU (supported by the amdgpu driver) is required. NVIDIA- or Intel-only systems are not supported for LACT features.

## About this fork
This repository builds a ChimeraOS-based image with LACT (Linux AMD Control Tool) pre-integrated and ready to use on AMD GPUs.

What’s included:
- LACT installed from AUR (package: `lact-bin`).
- The `lactd` system service is enabled to start automatically on boot, so tuning applies in both Desktop Mode and Game Mode.
- The LACT GUI launcher (`lact.desktop`) is kept visible in GNOME Desktop Mode.
- AMD overclocking controls are unlocked by default via kernel parameter: `amdgpu.ppfeaturemask=0xffffffff`.
- A default LACT configuration is shipped at `/etc/lact/config.yaml` with:
  - `apply_on_boot: true` so saved settings are applied on startup.
  - `save_on_change: true` so changes made in the GUI persist automatically.

## How to use this fork (frzr-deploy)
- Install ChimeraOS using the official installer from the website linked above.
- Boot into ChimeraOS, connect to the internet, and switch to Desktop Mode (GNOME) or open a TTY.
- Check your current image status:
  - `frzr status`
- Deploy this fork using the official frzr-deploy command (replace placeholders):
  - `sudo frzr-deploy MY_GITHUB_USER/MY_FORK:RELEASE_CHANNEL`
  - Example: `sudo frzr-deploy DodoKandrac/steamos-lact:stable`
- Reboot to boot into the deployed image.
- Optional: if you need to roll back to the previous image:
  - `sudo frzr rollback`

## Quick start (Desktop Mode)
1. Open the LACT app from the GNOME app grid (search for "LACT").
2. Adjust fan, power, clocks, or voltage as desired.
3. Save your profile. Because this image enables apply-on-boot, `lactd` will reapply it on future boots.

Notes:
- No overclock is forced by default. Controls are unlocked, but your GPU will run at stock until you save a profile.
- Settings apply system-wide. In Game Mode, the `lactd` daemon runs and your saved profile remains in effect.

## Verify on a running system
- Package is installed:
  - `pacman -Qi lact-bin`
- Service is enabled and active:
  - `systemctl is-enabled lactd` (should print: enabled)
  - `systemctl status lactd`
- Desktop launcher exists:
  - `ls /usr/share/applications | grep -i lact`
- OC interfaces are exposed (indicates kernel parameter is active):
  - `test -e /sys/class/drm/card0/device/pp_od_clk_voltage && echo OK || echo MISSING`

## Safety and compatibility
- Overclocking/undervolting capabilities vary by GPU/APU and vendor firmware. Start with conservative changes and test stability.
- Some laptops/APUs may provide limited controls even with `ppfeaturemask` set.
- To disable auto-apply quickly: set `apply_on_boot: false` in `/etc/lact/config.yaml` and restart `lactd`.

## Build status and downloads
This repo is not installation media. To download official installation media, use the ChimeraOS website linked above.
