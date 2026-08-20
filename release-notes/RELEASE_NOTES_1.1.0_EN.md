# DisplayFormatManager 1.1.0

**21 August 2026 · macOS 13 Ventura or later · Apple Silicon Mac**

DisplayFormatManager 1.1.0 is the first major update after the public 1.0.0 release and also incorporates the work completed throughout the intermediate 1.0.1 builds.

This version focuses mainly on four areas: **macOS HDR/SDR synchronization**, **more reliable format and Scaling changes**, an **HDR/EDR Test Card**, and a **new persistent-profile lifecycle model**.

## HDR/SDR truly synchronized with macOS

In the Pro edition, switching between SDR and HDR no longer means selecting only a physical video format capable of carrying HDR.

DisplayFormatManager now also synchronizes the **actual per-display HDR/SDR state used by macOS**, so changes made by the app are reflected by the HDR control in System Settings and by the WindowServer HDR/EDR rendering pipeline.

In practice:

- SDR → HDR also enables the macOS HDR state;
- HDR → SDR correctly disables it;
- HDR → HDR and SDR → SDR transitions do not unnecessarily disturb an already-correct semantic state;
- timeout and **Restore** return the display to its original macOS HDR/SDR state as well;
- macOS semantic state and the physical video format remain separate and independently verifiable.

No luminance value, EDR headroom value or display model is hardcoded: DisplayFormatManager uses what macOS and the display actually expose.

## More robust format, refresh-rate and Scaling engine

Version 1.1.0 consolidates the application engine developed and validated through the intermediate builds following 1.0.0.

In particular, it fixes cases where macOS could keep the newly requested refresh rate while restoring the previous sampling format or bit depth after a format change.

When **video format and Scaling are changed together on the same raster**, DisplayFormatManager now uses a deterministic sequence:

1. apply Scaling through Core Graphics;
2. apply the exact final physical format;
3. verify the configuration that is actually active.

Rollback follows the same principle to restore the original HDR/SDR state, format, refresh rate and Scaling in a coordinated way.

Core Graphics is involved in video-format transactions only when the Desktop/Scaling mode actually needs to change, avoiding recommits that could interfere with the physical format that was just applied.

## Requested vs actually active configuration verification

DisplayFormatManager now explicitly verifies that the configuration actually applied matches the requested target.

If macOS settles on a different configuration:

- the interface reports the mismatch;
- the configuration actually detected is shown;
- **Keep** remains disabled, preventing a different mode from being accepted as if the requested one had succeeded.

Reports can also include **Target**, **Observed** and verification status while a protected transaction is active.

## 30-second safety timers

The confirmation window for protected changes increases from 20 to **30 seconds**.

The offer to create a new persistent profile after **Keep** also remains available for 30 seconds.

If the mode is not confirmed, the timeout continues to trigger automatic rollback to the original configuration.

## New “HDR Brightness” Test Card

The Test Card now includes a new **HDR Brightness** view, separate from the existing SDR test patterns.

This view uses a dedicated floating-point EDR rendering surface and can display levels above the `1×` SDR reference white when macOS exposes additional headroom.

The Test Card:

- detects the current EDR headroom dynamically;
- distinguishes `1×` SDR reference white from higher EDR levels;
- never exceeds the headroom actually available;
- adapts the rendering path to the current signal family;
- shows an informational state when the available headroom is `1×`, without treating that value as an error.

The existing SDR views remain separate from the HDR rendering path.

The lower control layout, margins and several runtime warnings observed during development have also been cleaned up.

## Improved sampling guidance

The Test Card **Sampling** view now provides contextual guidance for:

- RGB / YCbCr 4:4:4;
- YCbCr 4:2:2;
- YCbCr 4:2:0.

The guidance explains the kind of chroma-detail loss that can be expected without treating any single resulting tint as a universal pass/fail verdict.

## More precise connection and refresh-rate information

The sidebar and Test Card now use a clearer refresh-rate presentation.

When the nominal value differs from the underlying technical value, DisplayFormatManager shows both, for example:

`24 Hz (23.976 Hz)`

Values that do not need nominal rounding remain compact.

Connection detection has also been expanded to use the **final connector type declared by macOS**, when available — for example HDMI, DisplayPort, DVI or VGA — while keeping the internal connection identity used for persistence separate.

The Pro report can also include connection-path information such as source port, transport, declared downstream connector and available intermediate-device details.

## HDR/EDR reports

Reports remain snapshots of the display's **current state**, not histories of changes performed by the app.

The Base edition adds the current macOS HDR status.

The Pro edition can additionally report:

- macOS HDR support;
- current macOS HDR enabled/disabled state;
- available EDR headroom where applicable;
- physical video format separately from semantic HDR state.

Keeping these layers separate makes mixed states visible instead of hiding them.

## Persistent profiles redesigned

Persistent-profile behavior has been significantly redesigned.

A profile is no longer continuously enforced in order to fight every manual user change. Instead, it is **re-armed by major display and system lifecycle events**:

- physical display reconnection;
- wake from sleep;
- a new graphical login session;
- macOS reboot;
- explicit profile reactivation.

On each event, the saved complete state is applied once. After a successful application, manual changes made by the user remain free for the rest of the current session.

If the display stack is not ready yet, the profile is not considered handled and the LaunchAgent can retry on a later pass.

Profile-to-display matching remains deliberately conservative: when identity is ambiguous, DisplayFormatManager avoids deliberately applying the profile to the wrong display.

## New profile states

Persistent profiles can now show five distinct states:

- **Current** — the saved target matches the current configuration;
- **Paused** — the profile is enabled, but the current session contains a manual override;
- **Suspended** — the profile has been deliberately disabled and will not act on lifecycle events;
- **Display not connected** — the associated display is currently unavailable;
- **Inconsistent** — the profile cannot be safely and uniquely associated with a display.

**Keep** no longer automatically suspends an already-enabled persistent profile: the kept configuration becomes a temporary session override and the profile moves to **Paused**.

Explicit actions are available to suspend and reactivate profiles. A paused profile can be reactivated immediately, deliberately ending the current session override.

The **Inconsistent** state uses an adaptive monochrome appearance: black in Light Mode and white in Dark Mode, with an automatically contrasting badge.

Imported profiles continue not to be automatically applied without an explicit user action.

## Mode catalogue settling after a change

After the requested target has been detected, DisplayFormatManager continues reading fresh display snapshots for a short settling period.

This allows WindowServer to republish related modes and timings after the modeset without performing a second modeset, restarting the timer or changing the transaction order.

## Base and Pro

The distinction between the two editions remains the one introduced with 1.0.0.

**DisplayFormatManager Base** continues to focus on inspection and format control within the scope allowed by the edition, with local profiles, Test Card, reports and protected rollback.

**DisplayFormatManager Pro** adds control across timings and configurations, SDR/HDR switching, advanced Scaling features, Adaptive Sync / VRR control and `.dfmprofile` import/export.

The new macOS HDR status information is also available in Base where applicable; SDR/HDR switching remains a Pro feature.

## Compatibility

DisplayFormatManager 1.1.0 requires:

- **macOS 13 Ventura or later**
- **Apple Silicon Mac**

The actual availability of formats, sampling modes, HDR, EDR headroom, high refresh rates and Adaptive Sync / VRR depends on the display, the connection being used and what macOS actually exposes.

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
