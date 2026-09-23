# Suggested issue: DELTA 3 Ultra AC Plugged In uses the wrong telemetry field

Submitted as [`rabits/ha-ef-ble#517`](https://github.com/rabits/ha-ef-ble/issues/517).

## Kind

Wrong value (sensor exists but is incorrect)

## Device

EcoFlow DELTA 3 Ultra, serial prefix `D751`. Firmware version still needs to be
recorded before submission.

## Sensor

AC Plugged In

## Description

On this DELTA 3 Ultra, `plug_in_info_ac_charger_flag` is not present in the
device heartbeat, so the existing family mapping leaves AC Plugged In unknown.
The unit reports mains presence through `plug_in_info_ac_in_flag` instead.

With AC connected, the observed value was `1`; removing AC changed it to `0`;
restoring AC changed it back to `1`. A DELTA 3 Ultra-only override using
`plug_in_info_ac_in_flag` produced the expected Home Assistant transitions
`on -> off -> on` during a non-critical lamp test.

This sensor is required for safe grid-loss alerts and recovery automations.

## Integration version

Reproduced on `v1.1.2`; current development commit
`89fa21c113f38640b65fdaa9329a918a14d9efd3` retains the original family
mapping.

## Diagnostics

Attach an EcoFlow BLE diagnostics dump captured while AC input is connected,
plus another after AC is removed if the issue form permits multiple files.

## Proposed change

Add a `Delta3Ultra.Device` override:

```python
plugged_in_ac = pb_field(delta3.pb.plug_in_info_ac_in_flag)
```

The prepared patch includes a unit test where `ac_in_flag=1` and
`ac_charger_flag=False`.
