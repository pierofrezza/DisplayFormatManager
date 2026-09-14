# DisplayFormatManager 1.2.0

**14 September 2026 · macOS 13 Ventura or later · Apple Silicon Mac**

DisplayFormatManager 1.2.0 significantly expands video-connection diagnostics and improves HDR format recognition, with a particular focus on **Dolby Vision**, **DisplayPort**, **link bandwidth** and **Display Stream Compression (DSC)**.

This version also makes the distinction between the format carried over the DisplayPort link and the format ultimately received by the downstream display much clearer, providing a more accurate description of connection paths that include adapters, docks and DisplayPort → HDMI bridges.

## Improved Dolby Vision support

Dolby Vision and Dolby Vision Low Latency are now correctly recognized as HDR formats.

Mode presentation has been refined to report more consistently:

- signal family;
- bit depth;
- range;
- HDR state;
- Dolby Vision variant, when identifiable.

Classification remains based on what macOS and the display actually expose, without turning a detected mode into a different format.

## More accurate HDR/SDR mode catalogue

Matching between SDR and HDR modes that share the same timings has been improved.

The comparison now takes greater account of raster, refresh rate and Fixed/Adaptive mode type, reducing the risk of associating configurations that belong to different categories.

When both families are available, the tabs are shown in this order:

`SDR | HDR`

and the active tab follows the category of the configuration that is actually in use.

## Expanded DisplayPort diagnostics

The Pro report now includes more detailed information about the DisplayPort link that is actually negotiated.

When available, it can report:

- link class;
- lane count;
- per-lane speed;
- link encoding;
- raw and usable bandwidth;
- estimated usage;
- remaining margin relative to the available capacity.

These values describe the connection that is actually active and remain separate from purely theoretical capabilities declared by the hardware.

## Live Display Stream Compression detection

DisplayFormatManager can now detect whether **Display Stream Compression (DSC)** is actually active on the current connection.

Detection is based on the live state of the connection rather than on a bandwidth estimate alone.

When DSC is active and the data is valid, the Pro report can show:

- DSC version;
- input bit depth;
- input chroma format;
- source and target bits per pixel;
- picture dimensions;
- slice dimensions;
- chunk size;
- compression ratio;
- estimated bandwidth after DSC;
- link usage and remaining margin.

When DSC is not active, the report states this explicitly.

The interface can also show a `· DSC` indicator when compression is actually active.

## DisplayPort source format separated from the final output format

In connection paths that include a bridge or adapter, the format carried on the DisplayPort side may differ from the format ultimately received by the display.

DisplayFormatManager 1.2.0 makes this distinction visible in the Pro report.

For example, a connection path may carry a DisplayPort YCbCr 4:4:4 10-bit signal with DSC upstream while delivering an HDMI YCbCr 4:2:0 10-bit signal downstream.

When available, the report therefore separates:

- final/downstream format;
- DisplayPort link format;
- bit depth;
- range;
- PQ/HDR state;
- link colorimetry.

This makes conversions performed by adapters or bridges visible instead of attributing them directly to the display or the Mac.

## Expanded HDMI diagnostics

Reading of HDMI capabilities reported through EDID has been expanded.

When present, the Pro report can include information related to:

- HDMI VSDB;
- HDMI Forum VSDB;
- TMDS limit;
- SCDC;
- FRL capabilities.

These values describe what the downstream display or device declares that it supports and remain separate from the live state of the connection.

## Interface improvements

Several parts of the interface have been refined to make modes and technical information easier to read.

In particular:

- Scaling mode cards adapt better to the available space;
- badges can wrap more naturally when space is limited;
- some DisplayPort labels have been made more precise;
- the DSC indicator uses a visual separator that better matches the rest of the interface;
- HDR/SDR tab ordering has been standardized to `SDR | HDR`.

## Faster Pro reports

Collection of the live connection information required by the Pro report has been optimized.

Connection data is reused within the same report-generation operation, avoiding unnecessary duplicate queries.

Report contents remain unchanged, while the time required to generate and save a report has been significantly reduced.

## Base and Pro

The distinction between the two editions remains the one established by previous versions.

**DisplayFormatManager Base** continues to focus on inspection and format control within the scope allowed by the edition, with local profiles, Test Card, reports and protected rollback.

**DisplayFormatManager Pro** adds control across different timings and configurations, SDR/HDR switching, advanced Scaling features, Adaptive Sync / VRR, `.dfmprofile` import/export and deeper technical connection diagnostics.

## Compatibility

DisplayFormatManager 1.2.0 requires:

- **macOS 13 Ventura or later**
- **Apple Silicon Mac**

The actual availability of formats, sampling modes, HDR, Dolby Vision, bit depths, high refresh rates, Adaptive Sync / VRR and DSC depends on the display, the connection being used, any adapters or docks in the path and what macOS actually exposes.

DisplayFormatManager does not create display modes that are not supported by the display.

> **Note:** DisplayFormatManager does not support managing Apple displays, whether built-in or external. Its analysis and control features are intended for compatible third-party external displays.

## Distribution

Both Base and Pro applications are:

- signed with an Apple Developer ID certificate;
- built with Hardened Runtime;
- notarized by Apple;
- distributed through DMGs that are also signed and notarized.

The official SHA-256 checksums for the DMGs are included in the `SHA256SUMS.txt` file attached to the release.

## Feedback

Displays, adapters, docks and connection paths can produce a very large number of combinations.

If you encounter unusual behavior, a particular configuration or something that could be improved, please report it through the project's **GitHub Issues**.
