# DisplayFormatManager 1.3.0

**22 September 2026 · build 183 · macOS 13 Ventura or later · Apple Silicon Mac**

DisplayFormatManager 1.3.0 completes a major development cycle focused on video-path diagnostics and external-display control, with a new interactive technical report, DPCD access, deeper EDID/CTA analysis, consolidated DSC detection and HDMI-CEC support on compatible paths.

The release keeps a conservative approach: low-level information is surfaced only when it can be associated with the selected display with sufficient confidence. Diagnostics remain read-only where appropriate, and DFM does not promote states or expose controls when the available evidence is insufficient.

## New technical report

The report has been completely reorganized.

Both editions can now:

- open the report in an **expandable/collapsible sheet**;
- expand or collapse all sections;
- save the report as TXT;
- obtain both sheet and TXT output from the same logical report source, preventing the two representations from drifting apart.

The EDID presentation has also been reorganized into a clearer structure: summary, declared capabilities, HDMI/CTA, timings, advanced diagnostics and raw data at the end.

## DPCD diagnostics

DisplayFormatManager Pro can read real DPCD information on compatible DisplayPort paths.

When available and safely attributable to the selected display, the report can include:

- DPCD revision;
- receiver capabilities;
- maximum link rate;
- maximum lane count;
- current link configuration;
- Enhanced Framing;
- per-lane CR / EQ / SYMBOL_LOCK state;
- interlane alignment;
- Extended Receiver Capabilities.

The read path is isolated, read-only and fail-closed.

## Display Stream Compression and macOS 26.7 compatibility

Live **Display Stream Compression (DSC)** detection has been consolidated after changes observed in IOAV APIs on macOS 26.7.

When DSC is active and the data is valid, the Pro report can show:

- DSC version;
- input bit depth and chroma format;
- source and target bits per pixel;
- picture and slice dimensions;
- chunk size;
- compression ratio;
- estimated post-DSC bandwidth;
- link usage and remaining margin.

Detection continues to use the live connection state rather than bandwidth estimation alone.

## Deeper EDID and CTA analysis

EDID retrieval has been expanded on DisplayPort and DisplayPort → HDMI paths, retaining IORegistry as the first choice and adding a read-only IOAV / DCPAVServiceProxy fallback where needed.

The structured parser can interpret, when present:

- EDID 1.3 and 1.4;
- detailed, standard and established timings;
- Display Range Limits;
- CTA Video Data Blocks;
- Audio Data Blocks;
- Speaker Allocation;
- HDR Static Metadata;
- Colorimetry;
- Video Capability;
- YCbCr 4:2:0 Capability Map;
- HDMI VSDB;
- HDMI Forum VSDB;
- declared TMDS, SCDC and FRL capabilities;
- AMD FreeSync VSDB v1.

Non-standardized or insufficiently documented blocks remain available as raw data without assigning unverified meanings.

## HDMI-CEC

HDMI-CEC handling has been consolidated on compatible paths that expose CEC-over-AUX.

When DFM can verify an operational endpoint, it can expose **Power** control to place the display in standby and wake it again.

The logic conservatively handles:

- CEC availability detection;
- Power state;
- standby and wake;
- hot-plug;
- DFM-initiated and external wake;
- source and logical-address recovery when needed.

Power control is not shown when the path or sink does not provide sufficient evidence of operational CEC support.

## HDR and MPDisplay

The passive MPDisplay dependency previously used during normal HDR detection has been removed.

HDR support is now derived from the mode catalogue and the active HDR state from the current mode. This removed the `bucketizeDisplayModes` warning observed on some configurations without reintroducing passive MPDisplay usage.

## Interface and localization

The release also refines:

- technical-sheet layout and readability;
- per-section dynamic property/value columns;
- DisplayPort terminology and indicators;
- EDID/CTA/HDMI presentation;
- Italian and English report strings;
- consistency between the visual report and TXT export.

Automatic development session logging has been removed from the release path.

## Release verification

Version 1.3.0 build 183 was subjected to a release-equivalent regression pass on:

- HP U28 4K HDR via direct USB-C to DisplayPort;
- HP U28 4K HDR via Apple A2119;
- HP U28 4K HDR via Belkin AVC003;
- Samsung HDMI via Belkin AVC003 for HDMI-CEC and MPDisplay regression coverage;
- Base and Pro editions on the main paths.

Testing covered startup/refresh, Test Card, scaling, rollback, HDR/SDR, DPCD, DSC, EDID/CTA/HDMI/FreeSync, sheet ↔ TXT consistency, report saving, absence of automatic session logging and HDMI-CEC behavior on validated paths.

## Compatibility

DisplayFormatManager 1.3.0 requires:

- **macOS 13 Ventura or later**
- **Apple Silicon Mac**
- **compatible third-party external display**

Actual availability of formats, sampling modes, HDR, Dolby Vision, bit depths, high refresh rates, Adaptive Sync / VRR, DPCD, DSC and HDMI-CEC depends on the display, connection path, adapters or docks in use and what macOS exposes for that specific configuration.

DisplayFormatManager does not create display modes that are not supported by the display.

> **Note:** DisplayFormatManager does not support managing Apple displays, whether built-in or external.

## Distribution

Both Base and Pro applications are:

- signed with an Apple Developer ID certificate;
- built with Hardened Runtime;
- notarized by Apple;
- distributed through DMGs that are also signed, notarized and stapled.

Official SHA-256 checksums:

```text
57ce02ae8d2b69aa30e548479f24034795b473487821744b718ceb94d37d31c2  DisplayFormatManager-1.3.0.dmg
9da34f0a9b2d67be2b0558d4c25a81b21c3dc91da58c7a6f26d5a1c667c9fa22  DisplayFormatManager-Pro-1.3.0.dmg
```

## Feedback

Displays, adapters, docks and connection paths can produce a very large number of combinations.

If you encounter unusual behavior, a particular configuration or something that could be improved, please report it through the project's **GitHub Issues**.
