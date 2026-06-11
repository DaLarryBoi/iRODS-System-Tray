# ADR 0001: Build a Minimal Windows-First iRODS System Tray Ingest App

## Status
Accepted

## Date
2026-06-11

## Context
The goal of this project is to build a minimal desktop background application that makes iRODS Automated Ingest usable for people who do not want to manage ingestion from the command line.

The application should behave like a Windows system tray utility that lets users:

- choose one or more local folders to watch
- keep monitoring active while the configuration window is closed
- detect new local files and queue them for ingest
- show simple status such as idle, ingesting, paused, or failed
- expose basic activity logging and pause/resume controls

This work is aligned with the direction of the `iRODS-System-Tray` concept and is intended as a mockup / proof of concept first. The tray application is a desktop workflow wrapper around the existing iRODS automated ingest capability rather than a replacement for it.

The main problem being solved is usability. iRODS Automated Ingest is powerful, but a CLI-first workflow requires users to understand configuration details, prepare jobs manually, and run commands correctly. The tray app shifts that experience from a shell workflow to a desktop workflow with continuous background monitoring and visible status.

Target users are primarily Windows users such as researchers, lab staff, and other institutional users who need to ingest local files into iRODS without depending on direct CLI usage.

## Decision
We will build the first version as a minimal Windows-first system tray application that wraps the existing iRODS automated ingest capability in a desktop workflow.

The initial product scope is:

- tray icon and context menu
- settings window for folder selection and monitoring controls
- background folder watching that continues when the window is hidden
- queue-oriented ingest workflow for newly detected files
- logging and visible status for recent activity and failures

The application will integrate with the existing iRODS automated ingest tooling instead of introducing a new ingest engine in the first iteration.

## Reasoning
- A tray-based desktop app directly addresses the usability gap between the power of automated ingest and the expectations of non-CLI users.
- A Windows-first proof of concept reduces initial scope while still targeting the main user environment described for this project.
- Keeping the first version minimal helps validate the workflow, queue model, failure handling, and operator experience before broader platform or implementation investments.
- Reusing the existing ingest capability reduces early technical risk and keeps the project focused on usability rather than reimplementing ingestion logic.

## Consequences
- The first release is intentionally focused on usability, tray behavior, monitoring, status visibility, and workflow simplicity.
- The application architecture should separate the tray/UI layer, folder watcher, ingest runner, config management, queue/state handling, and logging/status concerns.
- Monitoring must continue independently of whether the settings window is open.
- The product will initially depend on existing iRODS ingest capabilities and must define a clear integration boundary for invoking them.
- Specific implementation technology choices are recorded separately so they can evolve without rewriting the product-direction ADR.
- Future decisions about iRODS communication details, authentication flow, retry behavior, and packaging should be captured in separate ADRs.
