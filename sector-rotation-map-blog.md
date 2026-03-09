# How I Built a Free, Real-Time Sector Rotation Map — and Why Every Trader Needs One

*Reading time: ~7 minutes*

---

If you trade US equities, you already know that markets don't move in a straight line — and neither do sectors. Money rotates. Tech runs for six months, then financials take over, then energy. The traders who see that rotation early are the ones positioned ahead of the crowd.

The problem? Professional-grade sector rotation tools cost money. Bloomberg terminals are out of reach for retail traders, and most charting platforms either lock the Relative Rotation Graph (RRG) behind a subscription or give you a watered-down version with no customization.

So I built my own. A single HTML file, no subscription, no server, no database — just open it in your browser and it works.

Here's how it works, what it shows, and how you can use it.

---

## What Is Sector Rotation, and Why Does It Matter?

The US stock market is divided into 11 GICS sectors — Technology, Financials, Health Care, Energy, Consumer Discretionary, Consumer Staples, Industrials, Materials, Utilities, Real Estate, and Communication Services. At any given time, some sectors are outperforming the broader market and accelerating, some are outperforming but slowing down, some are underperforming but starting to recover, and some are in full retreat.

The classic framework for visualizing this is the **Relative Rotation Graph (RRG)**, originally developed by Julius de Kempenaer. The idea is elegant: plot each sector's *relative strength against a benchmark* on one axis, and the *momentum of that relative strength* on the other. What you get is a four-quadrant map that tells you — at a glance — where every sector sits in its rotation cycle.

The four quadrants are:

- **Leading** (top-right): Strong relative performance, and that strength is still building. This is where you want to be long.
- **Weakening** (bottom-right): Still outperforming, but momentum is fading. Time to watch closely.
- **Lagging** (bottom-left): Underperforming and losing momentum. Avoid or short.
- **Improving** (top-left): Underperforming but starting to recover. Early rotation opportunity.

Sectors naturally tend to rotate clockwise through these quadrants over time — leading to weakening to lagging to improving and back to leading. Spotting a sector early in the "improving" phase, before it crosses into "leading," is one of the highest-conviction setups in macro trading.

---

## Why Weekly Bars Change Everything

The most common mistake when building an RRG is using daily bars. Daily data looks clean on paper but produces a chart that looks like a bowl of spaghetti — 140+ data points per sector all stacked on top of each other, overlapping tails that are impossible to read.

Weekly bars are the industry standard for RRG analysis, and for good reason. Each dot on the tail represents one week of price action. An 8-week tail shows two months of rotation — enough context to identify a trend without so much noise that the signal disappears.

The tool defaults to an 8-week tail, but you can instantly switch to 1W, 4W, 12W, 16W, or 26W with a single click. Short tails (1W–4W) show current momentum. Longer tails (16W–26W) show the macro rotation arc.

---

## How the Math Works

For the technically inclined, here's what's happening under the hood.

**Step 1 — Relative Strength Ratio**

For each sector ETF, we calculate the ratio of its weekly close to the benchmark's weekly close, multiplied by 100. This gives us a raw relative strength index where 100 = exactly in line with the benchmark.

**Step 2 — Smoothing**

Raw ratios are noisy. We apply a 10-period Exponential Moving Average (EMA) to smooth the ratio, then calculate a Simple Moving Average of that smoothed ratio. The RS-Ratio is the EMA divided by its own SMA, multiplied by 100. Values above 100 mean the sector is outperforming; below 100 means underperforming.

**Step 3 — Momentum**

We repeat the same EMA/SMA process on the RS-Ratio series itself to get RS-Momentum. This measures whether the relative strength is accelerating or decelerating. Above 100 = building momentum; below 100 = fading.

**Step 4 — Plot**

RS-Ratio on the X-axis, RS-Momentum on the Y-axis. The center of the chart (100, 100) is where a sector is perfectly in line with the benchmark in both strength and momentum. The further a sector is from center, the more extreme its positioning.

---

## Features

**Live data, no API key required.** The tool fetches weekly OHLCV data directly from Yahoo Finance through a public CORS proxy. No registration, no billing, no rate limits for personal use. Data refreshes every time you open the file.

**Switchable benchmark.** The default benchmark is SPY (S&P 500), but you can switch to QQQ (Nasdaq 100), DIA (Dow Jones), IWM (Russell 2000), or VTI (Total Market) from a dropdown in the header. Switching the benchmark reloads all sector data and recomputes the entire chart against the new reference index.

**Add and remove sectors.** The default view shows all 11 GICS sector ETFs, but you're not limited to them. Hit "+ Add" in the sidebar, type any valid US ticker, and it fetches that ticker's data and plots it alongside the others. Want to add GLD to see how gold is rotating relative to equities? Add it. Want to compare QQQ to SPY on the same chart? Add it. Remove any sector with a single click.

**Persistent settings.** Your sector list, hidden sectors, tail length, and benchmark choice are all saved in your browser's local storage. Close the tab, reopen it tomorrow, and everything is exactly where you left it.

**Hover tooltips.** Mouse over any sector's head dot to see the exact RS-Ratio, RS-Momentum, current phase, and tail length. The sidebar shows the same data in a compact list format sorted by sector.

**Click to isolate.** Click any sector in the sidebar to hide it from the chart. Useful when you want to focus on a subset — for example, just the defensive sectors (XLV, XLP, XLU, XLRE) to assess whether the market is risk-on or risk-off.

---

## How to Read It in Practice

Here are the setups I look for in my daily workflow:

**The early improver.** A sector crossing from lagging into improving — moving left-to-right in the bottom half of the chart — is often the earliest signal of a rotation. At this stage, the sector is still underperforming, but its relative weakness is decelerating. Combine this with volume analysis (unusual volume in sector ETFs or individual names) and you have a high-conviction early entry.

**The confirmed leader.** A sector sitting firmly in the upper-right quadrant with a tail pointing up and to the right is in the strongest possible position — outperforming and accelerating. These are the sectors where momentum strategies have the best edge. Stay long, trail stops.

**The weakening leader.** A sector that has moved from leading into the weakening quadrant (tail curving down-right to down-left) is giving you a warning. It's still outperforming on a raw basis, but the edge is deteriorating. This is when I start tightening stops on longs and begin looking at the improving sectors for rotation candidates.

**Divergence.** When most sectors are in leading or improving and one is in lagging and falling further, that's a sector to avoid on the long side regardless of what individual names look like. Sector-level headwinds are a real drag.

---

## The Benchmark Matters More Than You Think

Most RRG tools default to SPY and leave it at that. But the benchmark you choose fundamentally changes the picture.

Against SPY, tech (XLK) often looks flat because tech is such a large component of SPY itself — the correlation is high, so the relative strength barely moves. Switch the benchmark to IWM (Russell 2000) and suddenly you can see how large-cap sectors are performing relative to small caps, which is a useful risk-on/risk-off signal. Switch to QQQ and you're measuring sector rotation within a growth-heavy context.

I typically run the chart twice — once against SPY for the standard macro view, and once against IWM when I'm trying to assess whether we're in a risk-on environment where small caps are leading large caps.

---

## Limitations to Be Aware Of

No tool is perfect. A few things to keep in mind:

The RRG is a *lagging* indicator by construction — it's built on moving averages of moving averages. Don't use it for intraday or even daily entries. Its value is in context and positioning, not timing.

The CORS proxy used for data fetching is a free public service. On rare occasions it may be slow or unavailable. If the chart fails to load, try refreshing after a minute.

The tool uses sector ETFs (XLK, XLF, etc.) as proxies for sectors. ETF composition changes over time, and some ETFs have quirks (XLC, for example, is heavily weighted to Meta and Alphabet). Keep this in mind when interpreting the chart.

---

## Technical Stack

For those curious about the implementation: the entire tool is a single HTML file with no build step, no npm install, no framework. It uses D3.js for chart rendering, vanilla JavaScript for all logic, and the Yahoo Finance chart API for data. Settings persist in `localStorage`. The RRG computation is implemented from scratch in pure JavaScript — EMA, SMA, RS-Ratio, and RS-Momentum — without any financial library dependencies.

The file size is under 25KB. It opens in any modern browser. You can host it on GitHub Pages, Netlify, or any static host in five minutes.

---

## Getting Started

Download the HTML file, open it in Chrome, Firefox, or Edge, and wait about 15–20 seconds for all 11 sectors to load. That's it. The first time it loads will feel slow — 12 separate API calls, one per sector plus the benchmark. Every reload after that is the same speed, but your layout and sector preferences are already saved.

From there, experiment with the tail length to calibrate your timeframe. I find 8W gives the cleanest read for swing trading setups (days to weeks), while 26W is better for understanding the macro cycle.

---

Sector rotation isn't a crystal ball, but it's one of the most durable signals in equity markets. Money flows from sector to sector in patterns that repeat across cycles. A tool that makes those patterns visible — without a Bloomberg terminal or a monthly subscription — is worth having in your toolkit.

---

*Built with D3.js and Yahoo Finance data. Single HTML file, runs entirely in the browser.*
