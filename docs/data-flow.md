# Data flow

| # | From -> to | Data | Purpose | Who can access |
|---|-----------|------|---------|----------------|
| 1 | Sense HAT -> `station.py` | Temperature, humidity, pressure | Measure the room | Processes on the Pi |
| 2 | `station.py` -> `data/weather.db` | Full observation: station ID, run ID, sample ID, UTC time, three values, validity, errors, context state, forecast, MQTT state | Keep readings if the network is down; feed the dashboard and model | Anyone with a login or file access on the Pi |
| 3 | `station.py` -> Mosquitto (`weather/station01/telemetry`, `/status`) | The same observation as JSON; `online`/`offline` status | Deliver readings to the PC | Any client that can reach port 1883 can read and publish |
| 4 | Mosquitto -> `central_receiver.py` | Telemetry, status and `history` messages | Archive on the PC | Same as 3 |
| 5 | `central_receiver.py` -> `data/central.db` | Validated readings, duplicates ignored | Central archive and model training data | Anyone with access to the PC account |
| 6 | `replay_history.py` -> Mosquitto `/history` | Saved Pi readings, not retained | Fill gaps after an outage | Same as 3 |
| 7 | `data/weather.db` -> `webapp.py` | Latest reading, context and forecast text, display state | Show current state | Processes on the Pi |
| 8 | `webapp.py` -> browser | HTML, JSON status, control form | Dashboard | The person with the SSH login |
| 9 | `train_model.py` -> `models/*.json` | Model numbers, test cases, report with a hash of the dataset | Shadow forecast | Anyone with the files. Test cases contain real readings |
| 10 | Model file -> Pi (`model_runtime.py`) | Model JSON | Run the forecast locally | Pi users |
| 11 | `station.py` -> `data/runtime.csv` | Run ID, sample ID, source, processing time | Performance evidence | Pi users |
| 12 | `inspect_data.py --export` | CSV of readings | Analysis or evidence | Wherever the file is copied |

## Could this data say something about a person?

- The measurements are environmental (on their own they are not personal data).
- Timestamps, a station ID and a room could together reveal when a space is occupied or heated.
- **Excluded uses:** attendance tracking, occupancy inference, monitoring individuals, and any health or safety warning.
