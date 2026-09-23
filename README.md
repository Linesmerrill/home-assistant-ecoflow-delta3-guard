# EcoFlow DELTA 3 Home Assistant community release

This folder contains the public, device-neutral parts of a live-tested EcoFlow
DELTA 3 Ultra Home Assistant setup. It excludes device serial numbers, account
details, notification targets, network addresses, and private screenshots.

## What the existing integration provides

[`rabits/ha-ef-ble`](https://github.com/rabits/ha-ef-ble) provides local BLE
telemetry and controls. As of development commit
`89fa21c113f38640b65fdaa9329a918a14d9efd3`, it does not include:

- the DELTA 3 Ultra AC-presence mapping validated on serial prefix `D751`;
- DELTA 3 Ultra Storm Guard active-state telemetry;
- the reserve schedule, Storm Guard priority, or AC-output recovery policies.

The protocol mappings belong upstream. The policy automations remain separate
Home Assistant blueprints so each owner can choose their times and battery
limits.

## Why blueprints instead of only a package

A Home Assistant package can group several automations in one YAML file, but
its entity IDs and notification targets must be edited by hand. Blueprints are
importable through the Home Assistant UI and present selectors for each user's
EcoFlow entities, times, limits, and optional notification actions. The
firmware-specific low-floor controller may become an advanced package after it
has been tested on more than one device and firmware version.

## Blueprints

### Reserve schedule with Storm Guard priority

[`blueprints/automation/ecoflow_delta3_reserve_storm_guard.yaml`](blueprints/automation/ecoflow_delta3_reserve_storm_guard.yaml)
implements:

- configurable weekday peak discharge to an elective reserve;
- grid hold after the peak window without writing reserve equal to current SOC;
- off-peak charging to the device's configured maximum;
- active Storm Guard priority and five-minute reconciliation;
- restoration of the normal discharge floor and current schedule after the
  event clears;
- a fail-safe pause when Storm Guard telemetry is unknown or unavailable.

It requires the proposed Storm Guard sensor from the upstream patch. Do not
publish an import link until that prerequisite is merged or clearly documented
for testers.

### AC output recovery guard

[`blueprints/automation/ecoflow_ac_output_guard.yaml`](blueprints/automation/ecoflow_ac_output_guard.yaml)
attempts to restore unexpectedly disabled AC output and reports whether the
attempt succeeded. It accepts optional user-defined notification actions.

## Upstream contributions

The proposed focused EcoFlow BLE changes and draft pull-request text are under
[`upstream/`](upstream/). They should be submitted as separate PRs:

1. Correct DELTA 3 Ultra AC-presence telemetry.
2. Expose read-only DELTA 3 Ultra Storm Guard telemetry.

## Validation boundary

The source installation passed these live transitions on one DELTA 3 Ultra:

- grid to weekday battery discharge;
- natural reserve to grid pass-through;
- Storm Guard full-battery grid pass-through;
- Storm Guard clear, permanent-floor restoration, and pre-peak reconciliation;
- recovery after an unexpected AC-output shutdown.

The public blueprint inputs still need live acceptance testing. Start with a
lamp or another non-critical load. The low-floor recovery controller remains
reference-only because its behavior is firmware-specific and has only been
tested on one unit.

See [`docs/TESTING.md`](docs/TESTING.md) for repeatable validation commands and
the current test record.

## Sharing path

Home Assistant can import automation blueprints directly from GitHub or a
GitHub Gist. After the upstream dependency and event-clear path are verified:

1. Publish this folder in a dedicated public repository.
2. Add stable `source_url` values to each blueprint.
3. Import each URL in **Settings → Automations & scenes → Blueprints**.
4. Post the release and test matrix to the Home Assistant Blueprint Exchange.

## License

Apache License 2.0. See [`LICENSE`](LICENSE).
