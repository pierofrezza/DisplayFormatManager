# DisplayFormatManager 1.4.0

**23 September 2026 · build 191 · macOS 13 Ventura or later · Apple Silicon Mac**

DisplayFormatManager 1.4.0 focuses on quicker everyday access to display status and common actions through a new optional macOS menu bar companion available in both Base and Pro.

The menu bar is deliberately designed as a **summary and quick-access surface**, not as a replacement for the main DisplayFormatManager window. Full display control remains in the application.

## macOS menu bar companion

Both editions can now optionally place DisplayFormatManager in the macOS menu bar.

The panel can show:

- connected displays;
- display name and a concise current-signal summary;
- HiDPI state when active;
- physical connection type, including HDMI and DisplayPort;
- compatible displays currently in standby;
- persistent-profile status;
- quick Test Card access;
- a button to bring the main application window to the foreground.

When no supported external display is currently connected, the panel shows a dedicated empty state instead of presenting inactive display controls.

## Connected displays and live synchronization

The menu bar and the main window share the same application state rather than maintaining independent display models.

As a result, the panel follows:

- display hot-plug and removal;
- startup with an already connected display;
- HDMI-CEC standby and wake changes;
- external wake events;
- persistent-profile state changes;
- Test Card state.

The display summary remains intentionally compact so that detailed format selection and technical diagnostics stay in the main application.

## HDMI-CEC quick wake

When a compatible display is in standby and DFM has already verified a working HDMI-CEC endpoint, the menu bar can expose an **On / Wake** action.

This does not introduce a separate CEC implementation: the menu bar uses the same already validated CEC state and control path as the main application.

Power control remains unavailable whenever the connection does not provide sufficient evidence of an operational CEC path.

## Test Card quick access

The menu bar includes a single **Test Card** menu listing the currently connected displays.

Test Cards remain independent per display, so multiple Test Cards can be open at the same time. The menu reflects their current state and can be used to open or close each one.

## Menu bar and login settings

A new settings control is available from the main-window toolbar.

It provides two options:

- **Show in Menu Bar**
- **Open at Login**

`Open at Login` is available only while `Show in Menu Bar` is enabled.

If launch at login is enabled, DisplayFormatManager starts as a menu bar utility without automatically opening its main window and without remaining visible in the Dock. Choosing **Open App** restores the normal application presence and opens the main window.

Disabling the menu bar also disables and unregisters launch at login, preventing an invisible background-only configuration.

## Interface refinements

The menu bar panel includes:

- the application icon and edition name in the header;
- a compact display overview with HiDPI and connection indicators;
- persistent-profile states matching the main application;
- Test Card on the left side of the footer;
- **Open App** on the right side;
- live Light/Dark appearance updates.

The menu bar status icon uses a dedicated display/control symbol with a heavier weight for clear identification.

## Technical report metadata

The shared technical-report document now explicitly includes:

- application version;
- build number.

Because the interactive sheet and TXT export use the same logical source, the information appears consistently in both representations.

## Release verification

Version 1.4.0 build 191 was subjected to final QA covering:

- Base and Pro editions;
- Italian and English localization;
- menu bar enable/disable behavior;
- application-setting persistence;
- startup with and without an external display;
- live hot-plug synchronization;
- empty-state presentation;
- Light/Dark appearance changes;
- multi-display Test Card handling;
- persistent-profile presentation;
- launch at login without automatic window/Dock presence;
- stable transition from menu-bar-only mode to the normal application window;
- HDMI-CEC standby and wake synchronization on Samsung HDMI via Belkin AVC003.

The previously validated 1.3.0 display-control, CEC and low-level diagnostic core was kept unchanged unless required by the new menu-bar presentation or application lifecycle.

## Compatibility

DisplayFormatManager 1.4.0 requires:

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
bde34c2c56523510bfc29dade279541a7f2e1546118ca3dca5b41a3e90241613  DisplayFormatManager-1.4.0.dmg
70cf20bfa0ac95c9d8603364f062a6fa8e51bb478e91498e30d1f5d3dc081b3a  DisplayFormatManager-Pro-1.4.0.dmg
```

## Feedback

Displays, adapters, docks and connection paths can produce a very large number of combinations.

If you encounter unusual behavior, a particular configuration or something that could be improved, please report it through the project's **GitHub Issues**.
