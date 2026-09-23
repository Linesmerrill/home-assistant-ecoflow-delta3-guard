# Expose DELTA 3 Ultra Storm Guard telemetry

## Summary

- map the existing DELTA 3 protobuf fields for Storm Guard enabled, active, and
  end time
- expose the active flag as a read-only safety binary sensor
- expose enabled and end time as attributes
- add a focused unit test for all three fields

## Hardware validation

During an active Storm Guard event on a DELTA 3 Ultra, the binary sensor was on,
the enabled attribute was true, and the end timestamp was populated. The
EcoFlow app simultaneously displayed its Storm Guard badge and the device held
Backup Reserve and battery charge at 100% in grid pass-through.

This PR intentionally adds no Storm Guard write control.

## Test

```text
python3 -m pytest -q tests/eflib/test_delta3_ultra.py
1 passed
```
