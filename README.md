# Street Games

A pair of browser-based games for testing your knowledge of city street names. Built with vanilla JavaScript and OpenStreetMap data — no install, no build step, just open and play.

**[Name It](#name-it)** — Click streets on the map and type their names.
**[Find It](#find-it)** — You're given a street name; find it on the map.

---

## Games

### Name It

An open-world exploration quiz. The map starts covered in fog. Click any fogged area to reveal the streets beneath it, then click a street and try to name it.

- Correct answers turn green and the name is labeled on the map
- Revealing an answer turns it red
- Fuzzy matching forgives typos and common suffix variations (Street/St, Avenue/Ave, etc.)
- Use **Clear Area** to drag-select a region and reset it for practice
- Your session is saved automatically — reload the page and pick up where you left off
- Export and import saves as JSON to back up or share progress

### Find It

A timed queue-based game. A street name appears in the side panel — find it on the map and click it.

- **Skip** puts the street back in the queue
- **Hint** pans the map and draws a circle around the street's general area
- **Reveal** shows you exactly where it is (counts as wrong)
- Score tracks correct vs. wrong answers across the session

---

## How to Play

No server required. Open `index.html` in a browser, or open either game file directly.

1. Enter a city name (e.g. `London, UK` or `Portland, Oregon`)
2. Click **Load** — streets are fetched from OpenStreetMap and cached locally
3. Play

The two games share a tile cache, so loading a city in one mode makes it immediately available in the other.

---

## Tech

- **Maps**: [Leaflet](https://leafletjs.com/) with CartoDB tiles
- **Street data**: OpenStreetMap via [Overpass API](https://overpass-api.de/)
- **Caching**: IndexedDB (persistent) + in-memory (session), keyed by map tile bounds
- **No frameworks, no build tools** — three self-contained HTML files

### Caching

The map is divided into \~6 km tiles. Street data is fetched per tile and stored in IndexedDB under the key `StreetQuizCache`. Subsequent visits (and switching between games) load from cache with no network requests. The app uses exponential backoff with four retries for Overpass API failures.

### Street Matching

Name It uses a two-pass matching algorithm:

1. Exact match after normalization (lowercase, strip punctuation)
2. Levenshtein distance within 20% of the answer length, with suffix stripping (`Market` matches `Market Street`)

---

## Project Structure

```
index.html          Landing page with links to both games
street-quiz.html    Name It game
street-finder.html  Find It game
```

Each file is self-contained with all CSS and JavaScript inline.
