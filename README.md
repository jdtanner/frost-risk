# ❄️ Frost Risk Checker

A 16-day air, ground and hoar frost risk forecast for any UK location. Designed for gardeners, smallholders and anyone protecting seedlings, tender plants or an unheated greenhouse.

**[Live site →](https://johnhilltanner.github.io/frost)** <!-- update URL as needed -->

---

## Features

- **16-day forecast** — reliable for the first 7 days; days 8–16 are shown with an accuracy warning
- **Three frost types assessed independently** — air frost, ground frost, and hoar frost
- **Unheated greenhouse estimate** — minimum inside temperature adjusted for wind speed and cloud cover
- **Tonight's summary banner** — at-a-glance risk level, min temp, wind, cloud cover, and greenhouse estimate
- **UK location lookup** — accepts full or partial postcodes (via postcodes.io) or place names (via Open-Meteo geocoding)
- **Use my location** — one-click geolocation via the browser
- **Shareable links** — copies a URL with coordinates so recipients get the same location without re-geocoding
- **Print support** — clean print stylesheet
- **Data freshness indicator** — shows when the forecast was last fetched, with a Refresh button
- **No tracking, no cookies, no data collected**

---

## How frost risk is assessed

Each day is scored across three frost types using minimum air temperature, wind speed, humidity, and cloud cover.

### Air frost
Purely temperature-based.
- **Likely** — min temp ≤ 0°C
- **Possible** — min temp ≤ 1.5°C

### Ground frost
Requires cold temperatures, still air, and clear enough skies for radiative cooling.
- **Possible** — min temp ≤ 0°C, or (min temp ≤ 3°C and wind < 9 mph and cloud cover < 60%)
- **Likely** — min temp ≤ 0°C, or (min temp ≤ 3°C and wind < 5 mph and cloud cover < 25%)

An overcast or breezy night at 2°C may see no ground frost at all.

### Hoar frost
A purely radiative phenomenon — requires sub-zero dewpoint, calm wind, and clear skies.
- **Possible** — dewpoint < 0.5°C and wind < 9 mph and cloud cover < 60%
- **Likely** — dewpoint < 0°C and wind < 6 mph and cloud cover < 25%

Dewpoint is calculated from min temperature and relative humidity using the Magnus formula.

### Overall risk level

Each day is scored and mapped to a risk level:

| Score | Level |
|---|---|
| ≥ 4 | 🔴 High |
| 2–3 | 🟡 Medium |
| 1, or any frost possible | 🟢 Low |
| 0, or min temp > 3°C | ⚫ None |

**Scoring:**
- Air frost likely → +3
- Air frost possible (not likely) → +2
- Ground frost likely (when no air frost) → +1
- Ground frost likely (always, if true) → +1
- Hoar frost likely → +1

A hard override forces the level to **None** when min temp > 3°C, regardless of other conditions.

### Greenhouse buffer

An unheated greenhouse is typically 1–4°C warmer than outside air. The estimated minimum inside temperature is calculated as:

```
base buffer:  wind > 12 mph → 1.5°C  |  wind > 6 mph → 2.5°C  |  calm → 3.5°C
cloud adjust: clear (< 25%) → +0.5°C |  overcast (> 60%) → −0.5°C
minimum floor: 0.5°C
```

On clear nights the outside air drops faster (radiative cooling), making the glass's insulating benefit larger relative to outside. On overcast nights the clouds already act as insulation, reducing the greenhouse's marginal advantage.

---

## Data sources

| Source | Used for |
|---|---|
| [Open-Meteo](https://open-meteo.com) | Weather forecast (temperature, wind, humidity, cloud cover, precipitation probability) |
| [postcodes.io](https://postcodes.io) | UK postcode geocoding |
| [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api) | Place name search |

All APIs are free and no API key is required.

---

## Tech stack

A single self-contained `index.html` file — no build step, no dependencies, no framework.

- Vanilla HTML, CSS, JavaScript
- Google Fonts (DM Serif Display, DM Sans)
- [Buy Me a Coffee](https://buymeacoffee.com) widget

---

## Deployment

Hosted on GitHub Pages. Any push to the main branch deploys automatically.

To run locally, simply open `index.html` in a browser. The geolocation and clipboard features require HTTPS or `localhost`.

---

## Planned

- **Email alert service** — daily frost warnings for subscribed locations, built with GitHub Actions (scheduler), Supabase (subscription storage), and Resend (email delivery)

---

## Licence

MIT — free to use, adapt and share.
