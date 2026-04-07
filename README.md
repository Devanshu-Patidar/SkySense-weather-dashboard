# SkySense Weather

A **vanilla** (no framework) weather dashboard: **HTML**, **CSS**, and **JavaScript**. Search by city or use your location, see current conditions and forecasts, get plain-language preparation tips, optional weather-style alerts, and a separate **graphs** page with temperature and humidity charts plus **air quality** and **Sun & UV** details. The UI is **mobile-friendly** and supports **light / dark** theme.

Repository: [github.com/Devanshu-Patidar/weather-dashboard](https://github.com/Devanshu-Patidar/weather-dashboard)

---

## Features

### Dashboard (`index.html`)

- **City search** with geocoding (supports queries like `City, IN`).
- **Use my location** via the browser geolocation API.
- **Current weather**: temperature, feels-like, humidity, wind, min/max, visibility, weather description.
- **Animated weather icons** using **Meteocons** (OpenWeather icon codes mapped to SVG assets).
- **5-day forecast** grid and a **12-hour** horizontal timeline.
- **UV index** (via [Open-Meteo](https://open-meteo.com/) — no separate API key) and **sunrise / sunset** times in local wall time for the selected city.
- **Preparation tips**: outfit, activity, and short mood-style lines derived from conditions.
- **Weather alerts** card: rule-based notices (e.g. rain/wind/storm-style messaging from live + forecast data) with optional **browser notifications** for stronger alerts (permission-based).
- **Theme toggle** (day / night) persisted in `localStorage`.
- **Open Weather Graph** button: opens `graph.html` with `lat` / `lon` query params for the last loaded location.

### Graph page (`graph.html`)

- **Sun & UV**: UV index (Open-Meteo, with hourly fallback if needed), sunrise and sunset times.
- **Charts** (Chart.js): **temperature** and **humidity** over the next window (interpolated / forecast-based); labels tuned for **small screens** (compact time labels, fewer point labels on narrow viewports).
- **Air Quality Index** from OpenWeather **air pollution** data, with a gradient scale and marker.
- **Theme toggle** aligned with the main app (stored theme).
- **Back to dashboard** link.

### General

- **Responsive layout** (including search and graph layout on phones).
- **Font Awesome** icons (CDN) and **Poppins** (Google Fonts).

---

## Tech stack

| Layer | Choice |
|--------|--------|
| Markup | HTML5 |
| Styling | CSS3 (custom layout, theme variables via `html.theme-light`) |
| Logic | JavaScript (ES modules not used; single-file scripts) |
| Charts | [Chart.js](https://www.chartjs.org/) + [chartjs-plugin-datalabels](https://github.com/chartjs/chartjs-plugin-datalabels) (CDN on graph page) |
| Icons (UI) | [Font Awesome 6](https://fontawesome.com/) (CDN) |
| Weather condition art | [Meteocons](https://github.com/basmilius/weather-icons) (MIT) — see `assets/weather-icons/LICENSE` |

---

## APIs used

| Data | Source | API key |
|------|--------|---------|
| Current weather, 5-day/3h forecast, geocoding | [OpenWeatherMap](https://openweathermap.org/api) | **Required** (`config.js`) |
| Air pollution (AQI inputs) | OpenWeatherMap Air Pollution | Same key |
| UV index | [Open-Meteo](https://open-meteo.com/) Forecast API | **None** (client-side `fetch`) |

---

## Project structure (main files)

```
├── index.html          # Main app (hero + dashboard)
├── script.js           # Weather fetch, UI, alerts, forecast, theme
├── style.css           # Global + dashboard styles
├── graph.html          # Charts + AQI + Sun & UV
├── graph.js            # Charts, AQI, Open-Meteo UV, theme
├── graph.css           # Graph page styles
├── config.example.js   # Template: copy to config.js and set key
├── config.js           # Your OpenWeather API key (see below)
├── .gitignore          # Ignores local config.js (optional); see note below
└── assets/
    └── weather-icons/    # Meteocons SVG sets (openweathermap/, all/, …) + LICENSE
```

---

## Configuration (OpenWeather API key)

1. Create a key in the [OpenWeatherMap dashboard](https://home.openweathermap.org/api_keys).
2. Copy `config.example.js` to **`config.js`** in the project root.
3. Set:

   ```js
   window.__OWM_API_KEY__ = "YOUR_KEY_HERE";
   ```

4. Ensure `index.html` and `graph.html` load **`config.js` before** `script.js` / `graph.js` (already wired in this repo).

**Alternative (dev only):** in the browser console:

```js
localStorage.setItem("OWM_API_KEY", "YOUR_KEY_HERE");
```

`.gitignore` lists `config.js` so a local file is not committed by mistake. If you deploy a static host that needs the key in-repo, you may commit `config.js` for your own deployment — **the key will be public** in the browser; use a **restricted** key and rotate it if it leaks.

---

## Run locally

- **Quick:** open `index.html` in a modern browser (some environments still prefer a local server for `fetch` / CORS).
- **Recommended:** serve the folder with any static server, for example:

  ```bash
  npx serve .
  ```

  Then open the printed URL and use the dashboard; open **Weather Graph** after loading a city so `graph.html?lat=…&lon=…` is populated.

---

## Deploy (GitHub Pages)

1. Push this repository to GitHub.
2. **Settings → Pages → Build and deployment:** source **Deploy from a branch**, branch **`main`**, folder **`/` (root)**.
3. After the site URL is live, add **`config.js`** on the deployed branch if the static host does not inject secrets (same public-key warning as above).

---

## Security and limitations

- All OpenWeather calls run **in the browser** with the key visible in DevTools / network tab. For production, prefer a **small backend** that holds the key and proxies requests.
- Open-Meteo is called directly from the client; respect their [fair use](https://open-meteo.com/) guidance.

---

## Credits

- **Meteocons** — Bas Milius, MIT License (`assets/weather-icons/LICENSE`).
- **OpenWeatherMap** — weather and air quality data.
- **Open-Meteo** — UV index.
- **Chart.js** and **chartjs-plugin-datalabels** — graph page.

---

## License

Project code: use and modify as you like for your own deployments; third-party assets remain under their respective licenses (notably Meteocons in `assets/weather-icons/`).
