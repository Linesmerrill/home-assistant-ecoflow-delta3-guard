# Suggested issue: expose DELTA 3 Ultra Storm Guard active state

## Kind

New sensor / control (value exists on the device but is not exposed)

## Device

EcoFlow DELTA 3 Ultra, serial prefix `D751`. Firmware version still needs to be
recorded before submission.

## Sensor

Storm Guard Active

## Description

The EcoFlow app shows when Storm Guard is actively preparing for a weather
event, but the DELTA 3 Ultra integration does not expose that state. The
existing `pd335_sys` heartbeat contains:

- `storm_pattern_enable`
- `storm_pattern_open_flag`
- `storm_pattern_end_time`

During a live Storm Guard event, `storm_pattern_enable` and
`storm_pattern_open_flag` were both true. The reported end timestamp matched
the event window shown by the device behavior. At the same time, the unit set
Backup Reserve to 100%, charged to 100%, and entered grid pass-through.

The active flag allows Home Assistant rate schedules to pause rather than
lowering Backup Reserve while the device is preparing for a storm.

## Integration version

Reproduced on `v1.1.2`; current development commit
`89fa21c113f38640b65fdaa9329a918a14d9efd3` exposes Storm Guard only for SHP3.

## Diagnostics

Attach an EcoFlow BLE diagnostics dump captured while the EcoFlow app displays
an active Storm Guard event.

## Proposed change

Expose `storm_pattern_open_flag` as a read-only safety binary sensor. Include
`storm_pattern_enable` and `storm_pattern_end_time` as attributes. Do not add a
write control in this change.
