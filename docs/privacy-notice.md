# Privacy notice: indoor weather station prototype

This is a student prototype. It is not a certified instrument, a health system or a production service.

**What it does.** A Raspberry Pi with a Sense HAT measures temperature, humidity and air pressure in one room every 5 seconds. It shows a state on an LED matrix and a local web page.

**What is recorded.** For each reading: the three values, the time (UTC), the station ID (`station01`), whether the reading was valid, the state it chose and why. It records no names, images, sound or location, and it is not meant to detect who is present.

**Where it is stored.** In a database file on the Pi, and in an archive on a PC if the receiver is running. Neither file is encrypted. The Pi's message broker also keeps the latest reading.

**Network.** If MQTT is on, readings travel from the Pi to the PC **without encryption**, and the lab broker accepts anyone on the network. The web page is only available on the Pi, or through an SSH tunnel.

**How long.** Database rows and logs older than **7 days** are deleted while the program is running. Exports, test files, backups and screenshots are deleted by hand at the end of the module.

**Your controls.** You can pause the LED alert for 60 seconds from the web page or the joystick. You can ask for the data to be deleted. `clear_retained.py` clears the broker's retained messages, but other copies need separate deletion.

**Limits.** The temperature reads high (about 36 C) because of the Pi's heat and is uncalibrated. The forecast is experimental and is not a weather forecast or warning.

**Questions.** Contact: **[to be completed: a real name and email]**

**Not decided.** The lawful basis, the responsible organisation and whether a data protection assessment is needed are unresolved. This notice makes no claim of legal compliance.
