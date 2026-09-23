# EcoFlow BLE upstream contribution drafts

These patches target `rabits/ha-ef-ble` development commit
`89fa21c113f38640b65fdaa9329a918a14d9efd3` from 2026-09-12.

They are intentionally separate:

- `0001-fix-delta3-ultra-ac-presence.patch` corrects an existing binary
  sensor mapping.
- `0002-expose-delta3-ultra-storm-guard.patch` adds read-only Storm Guard
  telemetry and a binary sensor.

Both add a focused DELTA 3 Ultra unit test. Each test passed under Python 3.14
in the live Home Assistant container on 2026-09-23.

The repository's contribution guide asks for a sensor-request issue and a
diagnostics dump before sensor work. The focused requests are now public:

- [DELTA 3 Ultra AC Plugged In uses the wrong telemetry field (#517)](https://github.com/rabits/ha-ef-ble/issues/517)
- [Expose DELTA 3 Ultra Storm Guard active state (#518)](https://github.com/rabits/ha-ef-ble/issues/518)

Diagnostics are still pending. Capture them while AC is connected or Storm
Guard is active, attach them to the matching issue, and wait for maintainer
direction before opening the PR.

Apply one patch to a clean checkout with:

```bash
git apply --check /path/to/0001-fix-delta3-ultra-ac-presence.patch
git apply /path/to/0001-fix-delta3-ultra-ac-presence.patch
python -m pytest -q tests/eflib/test_delta3_ultra.py
```
