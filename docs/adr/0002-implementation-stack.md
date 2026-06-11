# ADR 0002: Choose Python, PySide6, watchdog, and JSON for the First Implementation

## Status
Accepted

## Date
2026-06-11

## Context
ADR 0001 defines the product direction: a minimal Windows-first system tray application for watching local folders and ingesting data into iRODS through the existing automated ingest capability.

To deliver that proof of concept, the project needs an implementation stack that supports:

- a desktop GUI and tray icon
- a long-running background application lifecycle
- local filesystem monitoring
- a simple way to persist watched folders and application state
- fast iteration while the workflow and UX are still being defined

The team also discussed alternative directions including C++/Qt, `pystray`, `wxWidgets`, C#/.NET, and Rust-based options.

## Decision
We chose the following stack for the first implementation:

- Python as the implementation language
- PySide6 for the desktop GUI, tray integration, settings window, and application lifecycle
- watchdog for monitoring configured local directories
- JSON for storing local configuration such as watched folders and monitoring state

## Reasoning
- Python is the fastest way to produce a proof of concept and iterate on the user workflow, tray behavior, monitoring model, and ingest integration.
- PySide6 provides a mature Qt-based desktop framework with system tray support, native dialogs, widgets, and signal/slot patterns suitable for a responsive background desktop app.
- watchdog directly fits the requirement to watch local folders for file creation and related filesystem activity.
- JSON is sufficient for a small local desktop app that only needs to persist folders, basic settings, and monitoring state.
- This stack keeps the implementation close to the architecture of a potential future Qt/C++ version if later constraints justify a rewrite.

We considered but did not choose the following for the first version:

- C++/Qt: strong long-term option, but slower for a first mockup and proof of concept
- `pystray`: useful for very small tray utilities, but less suitable if the app also needs a richer desktop GUI and integrated application lifecycle
- `wxWidgets`: viable for cross-platform desktop apps, but Qt/PySide6 is a better fit for the desired tray + window integration and future extensibility
- C#/.NET native Windows: attractive for a Windows-only build, but less aligned with a possible cross-platform trajectory
- Rust-based desktop stacks: promising, but unnecessary complexity for the first implementation

## Consequences
- The team can validate the product quickly on Windows with relatively low implementation overhead.
- The codebase should be organized so tray behavior, monitoring, ingest execution, and state persistence remain cleanly separated.
- If later testing shows packaging, performance, or OS integration limits with Python, we can revisit a Qt/C++ implementation in a follow-up ADR.
- Technology changes can now be evaluated independently from the product-direction decision captured in ADR 0001.
