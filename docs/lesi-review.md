# Legal, ethical and societal review

Ratings are likelihood / impact (Low, Medium, High). A rating is a judgement, not proof that a risk is solved. **To do** marks checks I have not run yet.

| Concern | Who and what harm | What I did | What is left | Rating |
|---|---|---|---|---|
| **Privacy** | People in the room: timestamps could show when it is occupied | The ID is `station01`, only three measurements are kept, and `data-flow.md` lists excluded uses | A different room or label would change the risk | Low / Medium |
| **Security** | Data owner: someone on the network reads data or sends fake readings | The receiver rejects payloads over 16 KB and validates each reading. The dashboard is local-only, with CSRF protection, a Content-Security-Policy and a rate limit | The lab broker is anonymous and unencrypted. Files are unencrypted. No formal access test yet | Medium / Medium |
| **Transparency and control** | Dashboard users: being misled or unable to silence the alert | Every state has a reason. Stale or invalid readings show "Unavailable". Alerts can be paused for 60 seconds | The pause is fixed at 60 seconds and only affects the display | Low / Low |
| **Fairness and placement** | Anyone the sensor claims to represent: the Pi's heat makes it read about 36 C | Readings are labelled "uncalibrated" and the raw value is stored | No offset set. Placement notes and a thermometer comparison are **To do** | High / Low |
| **Reliability and over-trust** | Readers who take the demo for a warning service | The page says it is not a forecast or warning. The heat index only shows in its valid range. The model runs in shadow mode, abstains when unsure and is shown next to a "no change" baseline | The model has not been trained on real data | Medium / Medium |
| **Accessibility** | People who cannot see the matrix or use a pointer | Symbols plus words, not colour alone. The dashboard works offline and without a mouse | The matrix is small, the dashboard needs an SSH tunnel, and the keyboard, zoom and no-JavaScript checks are **To do** | Medium / Low |
| **Sustainability** | The owner: unbounded logs and constant sampling | 7-day retention, rotated logs, small measured workload | Power use not measured. Retention misses exports and backups | Low / Low |
