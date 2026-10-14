# V11 Manual Swing Signals — Android Research App v2

## What this version does
- Mobile-friendly, client-only research dashboard for manual decisions.
- Connects to Deriv's public WebSocket for active symbols, OHLC candles and tick updates. It requests no trading-account token and has no order-placement code.
- Includes Deriv synthetic-index symbols and searches Deriv's active-symbol response for forex/gold instruments when available.
- Ports the V11 source's core decision flow: normalized indicator features; regime gate; volatility gate; Trend and Momentum agent direction agreement; consensus threshold; context/experience qualification; and strategy discovery among trend-continuation, momentum-expansion and balanced playbooks.
- Implements the market-data adapter normalization formulas from `Adapters/MT5/MT5MarketData.mqh`: trend from EMA20/EMA50 separation normalized by ATR, momentum from RSI centred on 50, volatility from ATR/price, and regime from ADX/50. Browser code calculates ATR/RSI/ADX from public candles.
- `Record selected outcome` lets you record a manually reviewed outcome from -1 to +1. This learning data is stored only in that browser's local storage and is not synced with MT4/MT5.
- No demo values are used unless you explicitly tap `Load example only`.

## Fidelity limits (important)
This is a **browser port of the readable V11 source logic**, not the V11 EA running inside MT4/MT5. The app cannot access MT5's terminal Global Variables, the EA's running state, broker-specific indicators, or terminal-only learning history. Local browser outcome records are separate from V11's MT5 memory. This version does not claim the same output as a live EA instance, and it is not backtested or validated as profitable. Confidence is an internal decision score, not a probability of profit.

## Android setup
1. Download and extract the ZIP on Android.
2. Open `index.html` in a browser. If a `file://` or `content://` page cannot open the WebSocket, use a trusted HTTPS static host.
3. Choose `Deriv synthetic indices` or `Forex & gold`, select timeframe and tap `Connect / refresh data`.
4. A BUY/SELL appears only when the ported gates pass; WAIT means a gate blocked the setup or candles are insufficient. Review the reasons before making any manual decision.
5. For an installable home-screen shortcut, host over HTTPS and choose the browser menu → Add to Home screen / Install.

## Free data / hosting
Deriv documents public market-data calls for active symbols, ticks and tick history without authentication. The network/API may still fail, symbol availability can vary, and the app needs to remain open to receive live updates. This is not a 24/7 background monitor. Free hosting availability and terms can change; check the host's current rules. No VPS is included or required for an open browser session.

## API documentation
- https://developers.deriv.com/docs/data/active-symbols/
- https://developers.deriv.com/docs/data/ticks/
- https://developers.deriv.com/docs/data/ticks-history/
