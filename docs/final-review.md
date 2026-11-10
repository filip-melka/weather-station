# Final review

## What I built

A Raspberry Pi weather station. It reads temperature, humidity and pressure every 5 seconds, checks each reading, saves it locally and sends it to a PC over MQTT. It gives each reading a plain-language state (`normal`, `attention`, `unknown`, `warming_up`), runs a small forecast model alongside, and shows the state on the LED matrix and a local dashboard.

## What I tested and learned

- Unit tests cover bad values, duplicates, retention, history windows, the model and the dashboard.
- On the Pi, readings reached the PC about every 5 seconds, and the Pi kept saving when the link was down.
- The model was trained on simulated data, so the runtime refused to use it on real readings. That is the safeguard working, not a bug.
- Benchmark (one run): about 6 microseconds per prediction, about 45 ms for a full sample.

## Limitations

- MQTT is unencrypted and anonymous, and the data files are unencrypted.
- The temperature reads high (about 36 C) because the sensor sits next to the Pi, and it is uncalibrated.
- The model has not been trained on real data.
- Retention does not cover exports, backups or the broker's retained message.
- Some dashboard checks (keyboard, zoom, no JavaScript) and the placement test are still to do.

## Before I would deploy it

1. Encrypted and authenticated MQTT, plus encrypted storage.
2. A calibrated sensor in a documented spot.
3. A model validated on real data from the real location.
4. A named owner, a purpose, a lawful basis, a retention decision and a contact route.
5. Finished accessibility checks and a way to delete every copy of the data.

## Benefit, trade-off, open concern

- **Benefit:** it keeps working and keeps its data when the network is down.
- **Trade-off:** showing "unavailable" instead of an old number is less satisfying but more honest.
- **Open concern:** timestamps from a sensor in a shared space could reveal when people are present.

## Sources

Lecture topics: Legal, Ethical, and Societal Implications of AI at the Edge; Sensor Technologies; Middleware; Context-Aware Systems; Intelligence at the Edge; Ambient Intelligence; User Interface Design for Ubiquitous Computing. Documentation: Data Protection Commission, *principles of data protection*, *lawful bases* and *retention periods*.
