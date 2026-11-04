[README (1).md](https://github.com/user-attachments/files/34349697/README.1.md)
# 🎯 Bullseye

**A quick-reference stock snapshot built around your time horizon.**

Bullseye gives investors a clear, jargon-free read on a single stock in seconds. Instead of a wall of financial data, it shows only the metrics that matter for how long you plan to hold, explains every number in plain English, and boils it all down to one score.

**[Live demo](https://YOUR_USERNAME.github.io/YOUR_REPO/)** · [Report a bug](https://github.com/YOUR_USERNAME/YOUR_REPO/issues) · [Request a feature](https://github.com/YOUR_USERNAME/YOUR_REPO/issues)

<!-- Add a screenshot: ![Bullseye screenshot](docs/screenshot.png) -->

---

## Why Bullseye?

Platforms like Yahoo Finance are powerful, but they show everything at once. That can overwhelm new investors and slow down experienced ones who only want a few indicators. Bullseye takes the opposite approach:

- **One stock at a time.** No market dashboards, no index tickers, no noise.
- **Metrics matched to your timeline.** A short-term trader and a retirement saver care about different numbers.
- **Every metric explains itself.** Tap the ⓘ next to any number for a short, plain-English definition.
- **Zero wasted API calls.** The app loads no market data until you search for a ticker.

## Features

| Feature | Description |
| --- | --- |
| **Bullseye Score** | A 0 to 100 rule-based score with a verdict ("Bullseye!", "On target", "Close, but not quite", "Off target", "Missed") and a short written summary of the stock's strongest and weakest areas. |
| **Time-horizon modes** | Choose Short term, Medium term or Long term to load a tailored set of metrics. |
| **Custom metrics** | Build your own dashboard from 20 available metrics. Your choice is saved in your browser. |
| **ⓘ Explainers** | Beginner-friendly tooltips on every metric. |
| **Metric meters** | A color-coded bar under each metric shows at a glance whether the value looks healthy. |
| **Analyst consensus** | A visual breakdown of strong buy, buy, hold, sell and strong sell ratings. |
| **Latest headlines** | Recent news for the company, linked to the original source. |
| **Earnings heads-up** | On the Short term view, a notice appears when a company reports earnings within the next five business days, with the date, analyst estimates and recent beat/miss history. |
| **Light and dark mode** | Follows your device setting and can be toggled manually. |
| **Zero dependencies** | One HTML file. No build step, no framework, no backend. |

## Time horizons

| Horizon | Focus | Metrics |
| --- | --- | --- |
| **Short term** (days to weeks) | Momentum, volatility, trading activity | 5-day return, 3-month return, position in 52-week range, beta, average daily volume, P/E ratio |
| **Medium term** (months to a couple of years) | Growth, valuation, trend | 6-month return, revenue growth (1yr), EPS growth (1yr), P/E ratio, operating margin, position in 52-week range, current ratio |
| **Long term** (years) | Quality, profitability, balance sheet, income | Revenue growth (5yr), EPS growth (5yr), net profit margin, return on equity, debt/equity, price/sales, dividend yield, market cap |
| **My metrics** | Whatever you choose | Any of the 20 metrics |

## How the Bullseye Score works

The score is intentionally simple and transparent:

1. Each scoreable metric in the current view is converted to a value between 0 and 1 using a fixed range. For example, a P/E ratio near 12 scores well and one near 40 scores poorly, while 1-year revenue growth is scored from -5% to +25%.
2. Analyst consensus is added as one more input, weighted at 1.5 times a normal metric.
3. The inputs are averaged and scaled to 0 to 100.

| Score | Verdict |
| --- | --- |
| 75 to 100 | Bullseye! |
| 60 to 74 | On target |
| 45 to 59 | Close, but not quite |
| 30 to 44 | Off target |
| 0 to 29 | Missed |

Informational metrics (market cap, average volume, dividend yield and EPS) are displayed but have little or no effect on the score. The score changes with your selected time horizon because it only uses the metrics shown. The earnings notice is informational and does not affect the score.

> **Note:** The thresholds are general rules of thumb and do not adjust for sector. A P/E that looks high for a utility may be normal for a software company.

## Getting started

Bullseye is a static site, so there is nothing to install.

### Run locally

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

Then open `index.html` in any modern browser.

### Deploy with GitHub Pages

1. Push `index.html` to the `main` branch of a **public** repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**, then choose **main** and **/ (root)**.
4. Save. Your site will be live at `https://YOUR_USERNAME.github.io/YOUR_REPO/` within a couple of minutes.

## Configuration

Bullseye uses the [Finnhub](https://finnhub.io) API. API keys are defined near the top of the script in `index.html`:

```js
const KEYS = ['YOUR_FINNHUB_API_KEY'];
```

You can supply several keys. Bullseye rotates through them and automatically skips any key that returns a rate-limit error for 60 seconds, which raises your effective request capacity.

### ⚠️ Security notice

Because Bullseye runs entirely in the browser, **any API key in `index.html` is visible to anyone who views the page source.** Before publishing a public site:

- Use keys you are comfortable exposing, and rotate them if they are abused.
- Never commit paid or sensitive keys.
- For a production deployment, route requests through a small server-side proxy (for example a Cloudflare Worker) that holds the keys privately and caches responses.

## API usage and rate limits

A fresh stock lookup makes five requests to Finnhub:

| Endpoint | Purpose |
| --- | --- |
| `/quote` | Current price and daily change |
| `/stock/profile2` | Company name, logo, exchange and industry |
| `/stock/metric` | Fundamentals, returns, margins and ratios |
| `/stock/recommendation` | Analyst consensus |
| `/company-news` | Headlines from the past 7 days |

On the Short term view, one extra request to `/calendar/earnings` checks for upcoming earnings, and `/stock/earnings` is called only when a report is actually due soon.

Finnhub's free plan allows roughly 60 requests per minute **per key**, so a single key supports about 12 fresh lookups per minute across all users. Results are cached in memory for the duration of a session, so revisiting a stock or switching time horizons costs little or nothing. Check your Finnhub dashboard and terms for current limits and usage rules.

## Tech stack

- HTML, CSS and vanilla JavaScript (no frameworks or build tools)
- [Finnhub](https://finnhub.io) market data API
- [Sora](https://fonts.google.com/specimen/Sora) typeface via Google Fonts
- `localStorage` for saving theme, time horizon and custom metrics

## Project structure

```
.
├── index.html   # The entire application
└── README.md
```

## Roadmap

- [ ] Price chart
- [ ] Recent searches
- [ ] Side-by-side stock comparison
- [ ] Optional user-supplied API key field
- [ ] Server-side proxy with shared caching
- [ ] Sector-aware scoring

## Limitations

- Covers US-listed stocks available on Finnhub's free plan.
- Some metrics may show N/A for smaller or newer companies.
- Data may be delayed and is provided as-is by a third party.
- Earnings dates and estimates come from a third party and can change.
- The score is a simplified, rule-based estimate and not a prediction.

## Contributing

Contributions, issues and feature requests are welcome. To contribute:

1. Fork the repository
2. Create a branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m "Add your feature"`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a pull request

## Disclaimer

Bullseye is an educational tool and **does not provide financial, investment, tax or legal advice.** The Bullseye Score and any summaries are generated by simple rules from public data and may be inaccurate or out of date. Past performance does not guarantee future results. Always do your own research and consider speaking with a licensed financial professional before making investment decisions.

## Acknowledgments

Market data provided by [Finnhub](https://finnhub.io).

## License

Distributed under the MIT License. See `LICENSE` for details. *(Add a `LICENSE` file to your repository, or replace this section with the license you prefer.)*
