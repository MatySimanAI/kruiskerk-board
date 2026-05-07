---
name: Real-time bus departures at Kruiskerk, Amstelveen
description: How to fetch the next departing buses at the Amstelveen Kruiskerk stop in one shot
type: reference
---
User lives in Amstelveen. The bus stop "Kruiskerk" has two platforms (one per direction), but on drgl.nl they are bundled into a single **stop area** that already shows both directions:

- `NL:S:57243210` — Kruiskerk stop area (both directions, all lines)

Verified 2026-05-07: the same board returns 178 → Station Zuid *and* → Poortwachter, plus 357 → Elandsgracht *and* → Aalsmeer — i.e. both directions in one fetch.

Do NOT try `NL:S:57243220` — that ID returns an empty page on drgl.nl. The `…220` suffix that appears in the Connexxion URL is a *quay* id (`NL:Q:57243220`), not a drgl stop id, and there is no separate per-direction drgl URL.

## Fastest path (one tool call)

WebFetch this URL — it returns a clean live departure board parsed straight from operator feeds, both directions included:

```
https://drgl.nl/stop/NL:S:57243210
```

Prompt the WebFetch with: "Extract the next ~10 bus departures at this stop. For each: line number, destination, scheduled time, real-time/expected time, and any delay info."

## Lines that serve this stop

Connexxion / R-net 178, 357, 62; night buses N47, N57; sometimes 174, 186, 257.
Typical destinations: Elandsgracht Amsterdam, Aalsmeer Busstation, Station Zuid Amsterdam, Poortwachter Amstelveen.

## Fallbacks if drgl.nl is down

1. `https://9292.nl/` — search "Kruiskerk Amstelveen" → departures (URL pattern is dynamic, so navigate by search rather than constructing).
2. `https://www.connexxion.nl/en/results/bus-stop-information?stop={"quayid":"NL:Q:57243220","town":"Amstelveen","name":"Kruiskerk"}` — official Connexxion page; the page is JS-heavy and WebFetch often returns only chrome (no live times), so use only as a manual link to give the user.
3. Moovit page: `https://moovitapp.com/index/en/public_transit-Bushalte_Kruiskerk-Netherlands-site_22849581-101`

## Notes

- Present results in a table: Line | Destination | Time | Status.
- Confirm direction with the user before recommending a specific platform.
- Always include source links — these are live data sites and the user may want to refresh.
