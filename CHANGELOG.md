# Changelog

All notable changes to the Fellow Stagg EKG+ integration are documented here.
New entries are written by [release-please](https://github.com/googleapis/release-please):
the version follows the Conventional Commit messages (`feat:`, `fix:`, ...) merged
since the previous release, and the entry lists the pull requests merged since
then. Do not edit the version in `manifest.json` by hand;
merging the release PR bumps it, tags the release and publishes it. Releases up
to 0.7.7 were made in [samwiseg0/ha-stagg-ekg-plus](https://github.com/samwiseg0/ha-stagg-ekg-plus/releases).

## 0.8.1 (2026-11-07)

<!-- Release notes generated using configuration in .github/release.yml at main -->

## What's Changed
### Changes
* ci: lint workflows with zizmor and actionlint by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/13
* ci: scan the locked dependencies for known vulnerabilities by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/15
* ci: check PR titles and commit messages follow Conventional Commits by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/14
* ci: move the pull request check into its own workflow by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/17
* build(deps): override Home Assistant's uv pin with 0.13.4 by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/18
* fix: treat 0 C as the off reading in Celsius by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/19
* refactor: drive the keep-alive watchdog from the last notification time by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/20
* refactor(api): hold each frame until the next one proves it intact by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/21


**Full Changelog**: https://github.com/discreteRegister/ha-stagg-ekg-plus/compare/v0.8.0...v0.8.1

## 0.8.0 (2026-11-02)

<!-- Release notes generated using configuration in .github/release.yml at main -->

## What's Changed
### Changes
* docs: point the README and manifest at discreteRegister/ha-stagg-ekg-plus by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/3
* fix: keep an idle persistent link alive and report its availability by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/1
* ci: lock the dev environment with uv and require 100% coverage by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/7
* ci: automate releases with release-please by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/8
* ci: run on the merge queue and gate merges on a single CI passed check by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/11
* fix: ignore state frames spliced together by a lost notification by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/2
* ci: build the changelog from merged pull requests by @digitalresistor in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/12
### Dependencies
* ci: bump home-assistant/actions/hassfest from cd6f3635a740c84d8a14f6121a90068b8ece6bf6 to 86d4564712fa02543c808e3a693dd04f9d3f8361 by @dependabot[bot] in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/10

## New Contributors
* @digitalresistor made their first contribution in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/3
* @dependabot[bot] made their first contribution in https://github.com/discreteRegister/ha-stagg-ekg-plus/pull/10

**Full Changelog**: https://github.com/discreteRegister/ha-stagg-ekg-plus/compare/v0.7.7...v0.8.0

## 0.7.7

Now available directly in HACS.

### Changed
- Installation through HACS no longer needs a custom repository. The integration is in the HACS default list.

### Internal
- Dropped `bleak-retry-connector` from `manifest.json` requirements. Home Assistant core ships and pins that package, so declaring it here added nothing and could only conflict if the pins diverged.

### Notes
- No changes to options, entities, or the protocol. Existing installs upgrade transparently.

## 0.7.6

Entity-accuracy fixes and Bluetooth/diagnostics hardening, plus CI and test-quality improvements.

### Fixed
- The climate entity now reports **Idle** when the kettle has reached its target and is keeping warm, or when it is lifted off the base, instead of always showing **Heating** while powered on. It shows **Heating** only while the water is actively warming up.
- The **On base** binary sensor stays *Unknown* until the kettle reports its base state, instead of briefly claiming **On base** before any reading has arrived.

### Changed
- Before the first reading on a fresh connection, the climate slider now follows your Home Assistant temperature unit (°F/°C) instead of always defaulting to Celsius; once the kettle reports its own unit, that takes over as before.
- Diagnostics downloads now redact the kettle's Bluetooth address.
- BLE commands re-check the active connection on each write retry, so a reconnect that happens mid-retry can no longer send to a stale link.

### Internal
- Added a Ruff lint check to CI and pinned the test toolchain for reproducible builds.
- Corrected the downloadable release `.zip` layout so a manual install extracts into `custom_components/stagg_ekg_plus/` as expected.
- Added pip dependency updates to Dependabot and expanded entity, config-flow, and coordinator test coverage (148 tests, 100%).

### Notes
- No changes to options, entities, or the protocol. The one behavior change to be aware of is the more accurate climate action above: an automation that triggers on the climate entity's `hvac_action` being `heating` will now read `idle` while the kettle keeps warm at temperature. The **Holding temp** binary sensor remains the most reliable "at temperature" signal. Existing installs upgrade transparently.

## 0.7.5

### Fixed
- The power switch no longer momentarily appears as a definitive (non-assumed) state during a background poll. A background poll only briefly connects to check for a physical power-on, so the switch now stays "assumed" through it and is treated as definitive only during a genuine live session (kettle on, or persistent mode).

### Internal
- CI maintenance: updated GitHub Actions (checkout, setup-python, codecov) and pinned the HACS validation action to its maintained branch. No effect on the integration itself.

### Notes
- No changes to options, entities, or the protocol. Existing installs upgrade transparently.

## 0.7.4

Small correctness and code-quality improvements. No configuration changes.

### Changed
- The power switch now reports `assumed_state` while disconnected (in on-demand mode the shown state is the last known value and may be stale if the kettle was toggled physically); a live connection reflects the real state.
- Overridden entity and config-flow methods are now decorated with `typing.override`, matching current Home Assistant conventions and verified by strict type checking.
- Corrected an internal comment about the default temperature unit before the first state frame.

### Notes
- No changes to options, entities, or the protocol. Existing installs upgrade transparently.

## 0.7.3

Bluetooth reliability improvements, drawn from patterns in Home Assistant's `pysnooz` library. No configuration or entity changes.

### Changed
- The reconnect path now re-resolves the freshest `BLEDevice` on each internal retry (via `ble_device_callback`), instead of reusing a device captured once. This improves recovery on a busy or slow Bluetooth adapter.
- Commands are now serialized with a write lock so the switch and climate entities (separate platforms) can never overlap writes on the kettle's single characteristic.
- Transient BlueZ write errors (`org.bluez.Error.Failed` / `InProgress`) are retried with a short backoff before giving up; other errors surface immediately.

### Notes
- No changes to options, entities, or the protocol. Existing installs upgrade transparently.

## 0.7.2

A documentation and test-quality release. No user-facing behavior changes.

### Changed
- Reworked the README: a table of contents, shields.io badges (release, build, coverage, license, HACS), a consolidated feature/entity reference, and a condensed Connection mode section with less repetition.

### Added
- Codecov coverage reporting in CI.
- Full coordinator test coverage (100%); the suite now has 112 tests at 99% overall.

### Notes
- No changes to the BLE protocol, entities, or options. Existing installs upgrade transparently.
