# V11 Android Companion — v1 starter

## What this package does
- Responsive mobile-first dashboard that opens in a modern Android browser.
- Shows watchlist rows, side, model confidence score, sniper score, snapshot timestamp, rationale and recent events.
- Imports a JSON snapshot chosen by the user.
- Includes clearly labelled fictional demo data so the UI can be previewed without a broker connection.
- Can be installed as a PWA when hosted over HTTPS.

## Important limitations
- This is a dashboard starter, not an MT5 terminal and not a live connection to V11.
- The uploaded EA currently prints diagnostic events to the MT5 Experts/Journal logs; it does not provide a secure mobile API or the JSON export schema this dashboard expects.
- The demo rows are fictional. "Confidence" is not a probability of profit.
- This dashboard does not submit, modify or close trades.
- Do not put broker passwords, investor passwords, API keys, private keys or seed phrases into imported JSON.

## Open a local preview on Windows
1. Extract `V11_Android_Companion_v1.zip`.
2. Open the extracted folder.
3. Double-click `index.html` to preview in Edge or Chrome.
4. Press **Load demo preview** if the dashboard is blank or you want to reset the preview.
5. To import data, choose **Import V11 JSON** and select a JSON file matching the schema below.

Local preview is not the same as an installable PWA. Android's "Install app" option normally requires the site to be served over HTTPS.

## Install on Android (after hosting)
1. Publish the folder on a trusted HTTPS static host, such as a GitHub Pages site or another static web host.
2. Open the HTTPS URL in Chrome on Android.
3. Use Chrome's menu and choose **Install app** or **Add to Home screen** (wording varies).
4. The dashboard can then open like an app. It will still need a data-sharing bridge for live MT5 snapshots.

## JSON schema for import
Example (use real values only when supplied by an actual V11 export):

```json
{
  "schema": "v11-mobile-snapshot-v1",
  "generated_at": "2026-10-14T10:00:00Z",
  "source": "MT5 export",
  "connected": false,
  "account": {
    "currency": "USD",
    "equity": 0,
    "open_v11_positions": 0
  },
  "symbols": [
    {
      "symbol": "EURUSD",
      "timeframe": "H1 / H4",
      "side": "FLAT",
      "confidence": 0.0,
      "sniper_score": 0,
      "status": "NO ENTRY",
      "price": 0,
      "spread_points": 0,
      "reason": "Example only",
      "updated_at": "2026-10-14T10:00:00Z"
    }
  ],
  "events": [
    {
      "time": "2026-10-14T10:00:00Z",
      "level": "info",
      "title": "Example event",
      "message": "Example only"
    }
  ]
}
```

## Recommended next implementation stage: connect to MT5 safely
The EA needs a small bridge to export a sanitized status snapshot (symbol, timeframe, latest scan time, side, confidence, sniper score, reason, open V11 position count) to a controlled endpoint or a file that can be synced. A hosted dashboard alone cannot read the MT5 terminal's local files. The safe architecture is:
- MT5 EA on Windows/VPS → authenticated HTTPS backend → dashboard fetches sanitized data.
- Use a read-only API token, HTTPS, token rotation, request limits, and never expose trade-execution endpoints.
- Keep the initial dashboard read-only and test on a demo account before relying on it.
