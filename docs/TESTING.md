# Testing record

## Blueprint schema

Both blueprints were copied into an isolated Home Assistant configuration and
instantiated with placeholder entity IDs. Home Assistant accepted the complete
configuration:

```text
python3 -m homeassistant --script check_config -c /tmp/ha-blueprint-test
Testing configuration at /tmp/ha-blueprint-test
exit status 0
```

They were also placed in the live installation's blueprint directory without
creating automations from them. The live configuration check passed. This
proves schema and configuration validity; it does not count as a second live
power-transition test.

## EcoFlow BLE patches

Each proposed upstream patch was applied independently to development commit
`89fa21c113f38640b65fdaa9329a918a14d9efd3`. Its focused unit test ran under
Python 3.14:

```text
python3 -m pytest -q tests/eflib/test_delta3_ultra.py
1 passed
```

## Hardware evidence

One DELTA 3 Ultra with serial prefix `D751` provided the live evidence:

- `plug_in_info_ac_in_flag` followed controlled AC removal and restoration.
- Storm Guard enabled, active, and end-time fields populated during an event.
- Storm Guard held battery SOC and Backup Reserve at 100% while AC input and
  output matched in grid pass-through.
- When Storm Guard cleared at 13:00 local time, the live controller restored
  Min Discharge Limit from 0% to 1%, retained the pre-peak 100% reserve, and
  kept AC input and output matched in pass-through.
- The AC guard restored an unexpected device-side AC shutdown in about two
  seconds before its trigger delay was later removed.

## Before a public stable release

- Record the device firmware and Home Assistant versions.
- Attach an EcoFlow BLE diagnostics dump to each upstream sensor request.
- Run the public blueprints with a non-critical load.
- Ask at least one additional DELTA 3 owner to test with a different firmware.
