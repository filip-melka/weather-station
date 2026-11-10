# Retention

## What is kept and where

| Data | Why | Where | Who can remove it |
|------|-----|-------|-------------------|
| Readings, `data/weather.db` | Local record, dashboard, model | Pi | Anyone with Pi access |
| Readings, `data/central.db` | Central archive, training data | PC | Anyone with the PC account |
| Retained MQTT messages | Show the latest state to new subscribers | Broker on the Pi | Anyone who can publish, or `clear_retained.py` |
| `data/runtime.csv` | Timing evidence | Pi | Pi users |
| `data/demo.db` | Simulated practice data | PC | PC user |
| Model files in `models/` | Forecast and benchmark. The `*.cases.json` file holds copies of readings | PC and Pi | File owners |
| CSV exports, backups, screenshots | Evidence | Wherever copied | Whoever holds them |

## The seven-day setting

`retention_days` is 7 in `config.json`. It is an exercise choice. Seven days is enough for a few uninterrupted runs and for testing a one-minute forecast. It is not a recommendation for a real deployment. GDPR does not set one retention period for all data.

- `station.py` and `central_receiver.py` each purge about once an hour **while running**. Expiry is not instant.
- The station purge also deletes old `data/runtime*.csv` files.
- It does not cover exports, model case files, backups, screenshots or the broker's retained message.
- A database delete is not forensic erasure. Traces can remain in the file and on flash storage.

## Testing expiry

- `tests/test_core.py` checks that a temporary database drops an expired row and keeps a recent one.
- `python inspect_data.py --config config_sim.json --purge` removes expired rows from the simulated database only.

This shows that the purge logic works. It does not show that data is gone from disk, backups or copies.

## Deleting everything on purpose

1. Stop `station.py`, `central_receiver.py` and `webapp.py`.
2. Run `python clear_retained.py`, type the station ID, then check with a new `mosquitto_sub -t 'weather/station01/#'` that nothing arrives.
3. Delete the database files (including `-wal` and `-shm`), logs, exports and model case files on the Pi and the PC.
4. Remove backups and screenshots that show readings.

## What I keep

Only the documents, screenshots and benchmark numbers in `docs/`. Everything else is deleted at the end of the module.
