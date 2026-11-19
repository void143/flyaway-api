<!-- Publish this as the README of a PUBLIC GitHub repo, e.g. github.com/<you>/flyaway-api
     (or an org "flyaway-live"). It is a genuine, useful repo (API reference + client) AND a
     high-DA brand citation / sameAs target. Keep the brand description identical to the one in
     citation_targets.md everywhere. -->

# FLYAWAY GNSS-Interference API

**FLYAWAY ([flyaway.live](https://flyaway.live)) is an independent live map of GPS jamming and spoofing, built from the position-integrity that aircraft broadcast over ADS-B.** It is updated every few minutes and is advisory only, not a navigation instrument.

This repo documents the public GNSS-interference feed and shows how to consume it.

## What the data is
When GNSS is jammed or spoofed, aircraft report degraded position-integrity (NIC / NACp) over ADS-B. FLYAWAY aggregates that across all aircraft over an area into a real-time interference signal, per region and per map cell. Coverage follows air traffic, strongest across Europe, the Baltic, the Black Sea, the Caspian and the Middle East.

## Endpoints
Machine access needs an API key (`X-API-Key` header or `?key=`). Full reference: https://flyaway.live/api

```bash
# live interference, by region and map cell
curl -H "X-API-Key: YOUR_KEY" https://flyaway.live/api/v1/feed

# per-city daily history
curl -H "X-API-Key: YOUR_KEY" https://flyaway.live/api/v1/stats
```

## Example response (regions)
```json
{
  "generated": 1791704750,
  "regions": [
    { "name": "Riga",    "lat": 56.95, "lon": 24.10, "n": 765, "pct": 47 },
    { "name": "Vilnius", "lat": 54.70, "lon": 25.30, "n": 849, "pct": 41 }
  ]
}
```
`pct` = share of aircraft reporting degraded GNSS; `n` = aircraft sampled (confidence). Severity bands: LOW < 20, ELEVATED 20-39, HIGH 40-59, SEVERE >= 60.

## Minimal Python client
```python
import requests
KEY = "YOUR_KEY"
r = requests.get("https://flyaway.live/api/v1/feed", headers={"X-API-Key": KEY}, timeout=30)
for region in r.json()["regions"]:
    print(f"{region['name']:12} {region['pct']:3}%  ({region['n']} aircraft)")
```

## Use cases
Aviation and maritime situational awareness, drone pre-flight checks, OSINT and conflict monitoring, research into GNSS interference.

## Links
- Live map: https://flyaway.live
- Live data page: https://flyaway.live/stats
- API reference: https://flyaway.live/api
- Contact / evaluation key: founder@flyaway.live

## License
API responses are provided for evaluation and licensed use. Built from public ADS-B networks. Advisory only.
