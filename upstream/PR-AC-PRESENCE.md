# Fix DELTA 3 Ultra AC input presence mapping

## Summary

- override the DELTA 3 family AC-presence field for serial prefix `D751`
- use `plug_in_info_ac_in_flag`, which this model reports in its heartbeat
- add a focused unit test where the family `ac_charger_flag` is false

## Hardware validation

On a DELTA 3 Ultra, the patched entity followed a controlled non-critical-load
test as AC input was removed and restored: `on -> off -> on`. The original
mapping remained unknown because this firmware did not publish
`plug_in_info_ac_charger_flag`.

## Test

```text
python3 -m pytest -q tests/eflib/test_delta3_ultra.py
1 passed
```
