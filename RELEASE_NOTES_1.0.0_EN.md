# DisplayFormatManager 1.0.0

![DisplayFormatManager Pro 1.0.0](assets/screenshot-en.png)

**August 12, 2026 · macOS 13 Ventura or later · Apple Silicon Mac**

## Why DisplayFormatManager exists

DisplayFormatManager started from a very concrete problem: macOS does not always behave predictably when managing video formats for external displays.

In some configurations, especially when sampling, HDR, scaling, and refresh rate come into play, macOS may choose a different format from the one you expect, with visible effects such as banding or seemingly inexplicable changes in image quality.

While waiting and hoping for Apple to address these behaviors once and for all, I decided to **put a patch over the problem**.

That is how DisplayFormatManager started: first as a tool to understand what macOS was actually doing and explicitly choose the desired format; then, as development progressed, it grew into a full utility for controlling external displays.

## Features shared by both editions

DisplayFormatManager and DisplayFormatManager Pro share a set of features designed to make what macOS is actually doing with an external display more visible and more controllable.

Both editions can:

- show the video format and sampling actually being used by the display;
- analyze resolution, scaling, and refresh rate;
- show the display’s **physical connection** directly in the sidebar, such as HDMI or DisplayPort;
- select among the compatible formats available within the scope supported by the edition in use;
- apply a change temporarily and decide whether to keep it;
- automatically restore the previous configuration if a change is not confirmed;
- create and manage **persistent profiles**;
- identify the display associated with a profile and avoid automatic application when the display is unavailable or cannot be identified safely enough;
- use an integrated **Test Card** to visually check the result of the applied configuration;
- export a **report** containing the data detected by the app, limited to the information and capabilities available in the edition in use;
- organize information into **collapsible sections**, keeping the interface readable even when a display exposes many modes.

The goal is not to replace macOS Display Settings, but to provide an extra level of control precisely when the system’s automatic selection does not produce the desired result.

## DisplayFormatManager — Base

The Base edition is the core of DisplayFormatManager and is mainly designed to analyze and correct the format used by macOS **within the currently active timing**.

It can:

- show which format and sampling macOS is actually using;
- choose among the compatible formats available for the current timing;
- modify the supported configuration without changing the nature of the active timing;
- create local persistent profiles and keep them active over time;
- suspend and reactivate profiles without necessarily deleting them;
- show Adaptive Sync / VRR when it is active;
- use the Test Card and reports to verify and document the display configuration.

The Base edition **does not switch between SDR and HDR**.

If the active timing is SDR, changes remain within SDR; if the active timing is HDR, they remain within HDR.

Likewise, Adaptive Sync / VRR can be detected and displayed when in use, but **it is not directly controlled by the Base edition**.

Persistent profiles created with the Base edition remain **local to the app**: they can be created, activated, suspended, reactivated, and managed locally, but cannot be exported or imported.

## DisplayFormatManager Pro

**DisplayFormatManager Pro includes everything available in the Base edition** and adds a broader level of control over the complete display configuration.

In addition to the Base features, Pro can:

- work across different timings and configurations;
- switch between **SDR and HDR**;
- coordinate scaling, format, sampling, and refresh rate;
- filter scaling modes between **1× and HiDPI**;
- independently filter modes between **Standard and Advanced**;
- control **Adaptive Sync / VRR**, switching between fixed and variable refresh when supported by the display and connection;
- show more technical and detailed information about the display and available modes through the dedicated **eye** control;
- capture the display’s original state more completely;
- restore that configuration later;
- export and import profiles using `.dfmprofile` files;
- export multiple profiles in a single preset;
- selectively choose which profiles to import from a multi-profile preset.

Pro is therefore designed not only to correct the format currently selected by macOS, but to **define, preserve, transfer, and restore a complete display configuration**.

### Scaling: 1×, HiDPI, Standard, and Advanced

DisplayFormatManager Pro’s filters separate two different characteristics of the available modes.

**1× and HiDPI** identify the type of scaling:

- **1×** identifies modes without HiDPI scaling;
- **HiDPI** identifies modes where macOS renders at high density and scales the result to the display raster.

The second filter distinguishes between:

- **Standard** — modes normally exposed and used by macOS;
- **Advanced** — real, usable modes that DisplayFormatManager Pro can expose even though they are normally not shown in macOS Display Settings.

“Advanced” therefore does not mean experimental or inherently unsafe. It simply identifies a valid mode outside the selection normally presented by the macOS interface.

The two filters are independent: a mode can therefore be `HiDPI Standard`, `HiDPI Advanced`, `1× Standard`, or `1× Advanced`, depending on what the display and macOS actually make available.

## Persistent profiles and presets

Both editions can create persistent profiles to maintain a specific display configuration over time.

In the Base edition, profiles remain local to the app.

DisplayFormatManager Pro adds the ability to make those profiles **portable** using the format:

`*.dfmprofile`

You can:

- export a single profile;
- export multiple profiles in one file;
- import presets from another installation;
- choose, while importing a multi-profile preset, which profiles to import and which ones to ignore.

Imported profiles are initially kept suspended, so the user can decide when to activate them.

When persistence is enabled, DisplayFormatManager attempts to maintain the selected configuration even after changes made by the system or after the display is reconnected.

In Pro, restoring the original state can take into account the configuration that was actually present before the profile was activated, including resolution, scaling, SDR/HDR state, refresh rate, and, where applicable, the transition between fixed refresh and Adaptive Sync.

## Safe changes

Changing a video format can mean asking a display to switch to a configuration that differs significantly from the current one.

For this reason, DisplayFormatManager uses a timed confirmation mechanism where appropriate: the new configuration is applied and can be explicitly kept; otherwise, the app attempts to return the display to its previous state.

The app also avoids automatically applying a profile when the associated display is disconnected or cannot be identified safely enough.

## Test Card

Both DisplayFormatManager and DisplayFormatManager Pro include an integrated **Test Card**.

It can be used to quickly check the result of a configuration directly on the display, without relying on external images or tools.

It is especially useful while testing different formats to observe the display output and more easily spot differences, banding, or other visual anomalies.

## Reports

Both editions can export a **report of the detected configuration**.

The report naturally reflects the capabilities of the edition in use: Pro can include additional information and technical details related to the advanced features it can analyze and control.

Reports can be useful both as a snapshot of the display state and for documenting unusual configurations or anomalous behavior.

## Compatibility

DisplayFormatManager 1.0.0 requires:

- **macOS 13 Ventura or later**
- **Apple Silicon Mac**

> **Note:** DisplayFormatManager does not support managing Apple displays, whether built-in or external. Its analysis and control features are intended for compatible third-party external displays.

The actual availability of formats, sampling modes, HDR, high refresh rates, and Adaptive Sync / VRR depends on the capabilities of the display, the connection being used, and the modes macOS actually exposes.

DisplayFormatManager does not create unsupported display modes: it analyzes and controls what is genuinely available in the current configuration.

## Inspiration

DisplayFormatManager did not come out of nowhere.

During development I took inspiration partly from **BetterDisplay**, for the level of display control that can be useful on macOS, and partly from **WhatCable**, for its focus on connection information and on understanding what is actually happening between the Mac and the display.

In a way, DisplayFormatManager became a small synthesis of those two ideas: bringing connection information and format control together in one tool, while adding profiles, persistence, a Test Card, reports, and the other features developed throughout the project.

The goal was to keep the interface as **clear and approachable** as possible, even when the information being handled behind the scenes is considerably less friendly. 😁

## Distribution

Both the Base and Pro applications are:

- signed with an Apple Developer ID certificate;
- built with Hardened Runtime;
- notarized by Apple;
- distributed through DMGs that are also signed and notarized.

This allows Gatekeeper to verify the authenticity and integrity of the software before it runs.

## More than two months to reach 1.0

This first stable release arrives after **more than two months of development, analysis, experiments, and testing**.

A significant part of the work was not simply adding features, but understanding what macOS was actually doing behind the scenes: comparing formats, observing the behavior of different displays and connections, testing changes in sampling, scaling, HDR, refresh rate, and Adaptive Sync, and above all making sure that every change could be applied and undone without leaving the display in an inconsistent state.

Many of the situations explored during development normally remain completely hidden behind the standard system settings.

DisplayFormatManager started from a personal need, but the idea is that it may also be useful to anyone who has ever looked at a perfectly functional monitor and wondered:

**“Why did macOS decide to make it work differently today than it did yesterday?”**

If Apple fixes the underlying problem in the meantime, all the better.

Until then, there is DisplayFormatManager. 😁

## Feedback

Displays, adapters, docks, and connections can produce many different combinations.

If you encounter unusual behavior, a particular configuration, or something you think could be improved, you can report it through the project’s **GitHub Issues**.
