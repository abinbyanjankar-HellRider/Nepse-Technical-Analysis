# NEPSE Wyckoff Technical Analysis Dashboard

A live, self-updating single-page dashboard for Nepal Stock Exchange (NEPSE) technical analysis, built for PMS (Portfolio Management Services) use.

## Features

- **Wyckoff Method** — Full 7-task analysis: Phase ID, Volume-Price Action, Market Emotion, Key Levels, Trend & Structure, Trade Scenarios, Summary
- **Live Data via Claude API** — Auto-fetches today's NEPSE index, top gainers/losers, turnover, and NRB macro data on every open (Option C)
- **NPT-aware clock** — Correctly shows yesterday's close during market hours (11 AM–3 PM NPT), today's close after 3:45 PM NPT
- **Today's Price Table** — ShareSansar-style full stock price table with sort, filter, search, CSV export, and localStorage date database
- **Stock Analyzer** — Upload chart image → AI Wyckoff analysis with confidence scoring (P1 features: session persistence, validation engine, data gate)
- **NRB Macro Panel** — 60+ live fields from NRB Annual Report FY2025/26
- **Vertical sidebar navigation** — Candlestick-themed dark UI with green (bullish) / red (bearish) market aesthetics
- **Single HTML file** — No build step, no dependencies, works offline with fallback data

## Live Demo

🔗 [https://YOUR-USERNAME.github.io/nepse-dashboard](https://YOUR-USERNAME.github.io/nepse-dashboard)

## Data Sources

| Source | Data |
|--------|------|
| [nepalstock.com](https://nepalstock.com) | NEPSE index, top movers, market summary |
| [sharesansar.com](https://www.sharesansar.com/today-share-price) | Full daily price table |
| [nrb.org.np](https://www.nrb.org.np) | NRB policy rates, macro figures |
| [CEIC](https://www.ceicdata.com) | Monthly index series (verified anchors) |

## How the Live Update Works

On every page open, the dashboard calls the Anthropic Claude API with a web search tool. Claude searches `nepalstock.com`, `sharesansar.com`, and `nrb.org.np` for the latest NEPSE data and returns a structured JSON object. The dashboard parses this and re-renders all panels with live figures. If the API call fails, it falls back to the last known hardcoded data.

**API key**: The Claude API is accessed through Claude.ai's built-in API proxy when the file is opened from Claude.ai. No API key is required for basic use from within Claude.

## NEPSE Trading Hours (NPT = UTC+5:45)

| Time | Market State | Data Shown |
|------|-------------|------------|
| Before 11:00 AM | Pre-market | Previous day's close |
| 11:00 AM – 3:00 PM | **Market OPEN** | Previous day's close |
| 3:00 PM – 3:45 PM | Post-close / awaiting | Previous day's close |
| After 3:45 PM | ✅ Data available | Today's final close |
| Friday & Saturday | Weekend | Last Thursday's close |

## Wyckoff Key Levels (as of Aug 2026)

| Level | Price | Significance |
|-------|-------|--------------|
| Ice / SC Floor | 2,440 | Accumulation support — stop below |
| ST Zone | 2,550–2,600 | Secondary Test — key hold level |
| Creek / JAC | 2,838 | Resistance — breakout trigger |
| 2025 Cycle High | 2,983 | Prior peak |
| ATH | 3,079.8 | All-time high (Jul 2021) |

## Local Development

No build tools required. Just open `index.html` in any browser:

```bash
git clone https://github.com/YOUR-USERNAME/nepse-dashboard.git
cd nepse-dashboard
open index.html   # macOS
# or
start index.html  # Windows
```

## Deploying to GitHub Pages

See [DEPLOYMENT.md](DEPLOYMENT.md) for the full step-by-step guide.

## Project Structure

```
nepse-dashboard/
├── index.html          ← Complete dashboard (single file, ~315KB)
├── README.md           ← This file
├── DEPLOYMENT.md       ← GitHub Pages setup guide
└── .github/
    └── workflows/
        └── deploy.yml  ← Auto-deploy on push to main
```

## License

For internal PMS use at LS Capital Nepal. Educational purposes only. Not financial advice.
