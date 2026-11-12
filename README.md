# ParkWise 

> **AI-assisted parking app for smooth parking in Stockholm.**
> Know the rules, set a timer, avoid the fine.

ParkWise helps drivers understand the parking rules where they are standing — time limits, fees, restricted days and hours — and reminds them before their time runs out. The goal is simple: no more guessing at signs, no more parking tickets.

> **Status:** Early development (MVP). Stockholm only.

---

## Features

### MVP (v1)
- 📍 **Map view** — see parking spots around your current location
- 📋 **Rules at a glance** — tap a spot to see access, fees, max stay and time restrictions
- ⏱️ **Parking timer** — set a timer and get a reminder before your time expires
- 🔒 **Privacy first** — runs client-side; no location data stored on a server

### Planned
- 🤖 **AI rule assistant** — plain-language answers like *"Can I park here until 18:00?"*
- 🧠 **Smart suggestions** — nearby legal spots that fit how long you plan to stay
- 🔔 **Street-cleaning alerts** — warnings for recurring cleaning nights
- ⭐ **ParkWise Premium** — advanced features on a freemium model

---

## Tech Stack

| Layer          | Technology                        |
|----------------|-----------------------------------|
| Mobile app     | React Native (Expo) — in progress |
| Web MVP        | React (Lovable)                   |
| Maps           | Mapbox / Leaflet                  |
| Data source    | OpenStreetMap via Overpass API    |
| Data pipeline  | Go 1.22                           |
| CI/CD          | GitHub Actions + EAS Build        |

---

## Repository Structure

```
parkwise/
├── .github/workflows/
│   ├── ci.yml                 # Lint, test and build on every push
│   └── data-pipeline.yml      # Weekly OSM data refresh
├── data-pipeline/             # Go — OSM parking data fetcher
│   ├── cmd/main.go            # CLI entrypoint
│   ├── internal/osm/
│   │   ├── fetcher.go         # Overpass API client
│   │   └── parser.go          # OSM → ParkWise schema
│   └── output/                # Generated JSON (gitignored)
├── app/                       # React Native app (coming soon)
└── README.md
```

---

## Getting Started

### Prerequisites
- [Go 1.22+](https://go.dev/dl/)
- [Git](https://git-scm.com/)
- Node.js 20+ (for the app, once added)

### Run the data pipeline

```bash
git clone https://github.com/<your-username>/parkwise.git
cd parkwise/data-pipeline
go run ./cmd/... --city stockholm --output output/stockholm_parking.json
```

| Flag       | Default               | Description                         |
|------------|-----------------------|-------------------------------------|
| `--city`   | `stockholm`           | City to fetch (v1: Stockholm only)  |
| `--output` | `output/parking.json` | Output file path                    |
| `--pretty` | `true`                | Pretty-print JSON output            |

### Output schema

```json
{
  "city": "stockholm",
  "fetched_at": "2026-11-12T10:00:00Z",
  "count": 1234,
  "spots": [
    {
      "id": 123456789,
      "type": "node",
      "lat": 59.3293,
      "lon": 18.0686,
      "access": "public",
      "max_stay": "2 hours",
      "fee": false,
      "restrictions": [
        {
          "days": "Mo-Fr",
          "time_range": "08:00-18:00",
          "max_stay": "2 hours",
          "no_parking": false
        }
      ]
    }
  ]
}
```

---

## Roadmap

- [x] Product brief and naming
- [x] Go data pipeline (OSM → JSON)
- [x] Web demo (map + rules + timer)
- [ ] GitHub repo + CI/CD live
- [ ] Web MVP and landing page
- [ ] React Native app — map screen and rule display
- [ ] Timer + push notifications
- [ ] AI rule assistant
- [ ] Beta in Stockholm → App Store / Play Store

---

## Privacy & GDPR

ParkWise is built with privacy in mind:
- The MVP runs fully client-side.
- No user location data is stored or sent to a server.
- Any future server-side features will need explicit user consent.

---

## Data & Attribution

Parking data © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors, available under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/).

> ⚠️ ParkWise is a helper, not a legal authority. Always check the posted signs. Parking rules on the ground take precedence.

---

## Contributing

This is currently a personal project. Issues and suggestions are welcome — open an issue to start a discussion.

---

## License

[MIT](LICENSE) © 2026 Md Fatin Sirat
