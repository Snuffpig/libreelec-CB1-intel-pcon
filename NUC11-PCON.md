# Intel NUC 11 / DP-to-HDMI PCON Fork of LibreELEC CB1

This is a dedicated fork of [CroqueMr/libreelec-CB1](https://github.com/CroqueMr/libreelec-CB1) maintaining DisplayPort-to-HDMI protocol converter (PCON) support for Intel Gen11+ / Gen12 systems.

## What this fork adds

1. **Standard Dolby Vision over DP-to-HDMI PCON (`linux-9955-dp-pcon-dv.patch`, `linux-9984-dp-pcon-dv-bridge.patch`)**
   Standard (TV-led) Dolby Vision tunneling over an onboard DP-to-HDMI protocol converter on Gen11+ Intel graphics, plus `verified_bridge` support in `drm_dvbridge_validate_link()`.
2. **HBR Bitstream Audio and 7.1 LPCM (`linux-9956-pcon-hbr-audio.patch`, `0002-hda-intel-disable-iec958-preserve.patch`)**
   Fixes TrueHD 7.1 Atmos, DTS-HD MA, and multichannel LPCM playback across start, pause/resume, seek, stop/resume, and track switches on Intel Tiger Lake + Parade PS185 PCONs.
3. **10-bit HDR10 Settle (`linux-9957-dp-pcon-hdr10-10bit-settle.patch`)**
   Tracks PS185 10-bit mode entry (`pipe_bpp >= 30`) and waits out the 2000 ms settle window (or schedules a post-settle link retrain) so the converter does not drop the HDR10 DRM InfoFrame when switching from 8-bit SDR to 10-bit HDR10.
4. **4K50/4K60 10-bit YCbCr 4:2:0 HDR10 (`linux-9958-dp-pcon-hdr10-420.patch`)**
   Enables `ycbcr420_passthrough` and VSC SDP `colorimetry_support` on allowlisted PS185 PCONs and prefers 10-bit YCbCr 4:2:0 over 8-bit RGB when 4K50/4K60 HDR10 cannot fit in 10-bit RGB.

5. **Pending Upstream Kodi Fixes (`kodi-9998`, `kodi-9999`)**
   * `kodi-9998-fix-swig-action-richcompare.patch` ([`xbmc/xbmc#30822`](https://github.com/xbmc/xbmc/pull/30822)): fixes `xbmcgui.Action !=` comparisons under Kodi 22 SWIG `-builtin` (for example PM4K `OK` -> `Back` closing the OSD instead of stopping playback).
   * `kodi-9999-...-replace-truehdonly-with-pr30326.patch` ([`xbmc/xbmc#30326`](https://github.com/xbmc/xbmc/pull/30326) / [`CroqueMr/libreelec-CB1#310`](https://github.com/CroqueMr/libreelec-CB1/issues/310)): prevents false E-AC-3/DTS sync on TrueHD seeks.

## Verified hardware and support policy

* **Tested hardware:** Intel NUC 11 (`CM11EBV58W`, Tiger Lake i5-1145G7) with onboard Parade PS185 DP-to-HDMI PCON feeding a Sony Bravia 7 (eARC to Samsung Q990F).
* **Other Intel Gen11+ PCON boards:** `linux-9955` defaults `i915.dv_pcon=-1` (verified boards only). Other boards can test Standard DV by passing `i915.dv_pcon=1` on the kernel command line, though `linux-9956`, `linux-9957`, and `linux-9958` are currently DMI-gated to `CM11EBV58W`.
* **Maintenance:** Builds are compiled locally and published on a best-effort basis when upgrading personal hardware. GitHub Issues are disabled on this fork.
