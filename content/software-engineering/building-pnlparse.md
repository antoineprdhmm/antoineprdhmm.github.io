+++
title = "Building PNLParse: Design and Tradeoffs"
description = "PNLParse - Analyze trades and returns from broker files. Your data never leaves your machine."
date = 2026-01-27
+++

## Understanding the domain

Let’s start with a quick tour of the trading domain and the key concepts needed for this post.

When you trade, you execute transactions to buy or sell assets.

When you buy an asset you don’t already own, you open a new position. If you want to increase your exposure, you buy more and increase the size of that position.

If you want to reduce your exposure, you sell part of what you own, partially closing the position. When you sell everything, the position is fully closed.

Closing a position realizes P&L (profit and loss), based on how much you paid for the asset and how much you received when selling.

While you own assets, you have unrealized P&L, based on your entry price and current market prices.

Because positions can be increased or reduced over time, and because market prices change constantly, calculating P&L is more complex than it first appears. The same applies to portfolio performance analysis, where timing is critical.

## PNLParse

It’s not really possible to analyse trading performance directly on broker platforms.

Brokers usually don’t provide features for this. Their job is to make it possible to buy and sell assets. When using multiple brokers, the portfolio is split across platforms, which makes analysis even harder.

A common solution is to use Google Sheets or Excel. This is convenient because you can fetch market prices with functions like `=GOOGLEFINANCE` and use built-in formulas for calculations.

But it’s tedious to maintain and still requires work to build properly. Google Finance is missing many assets (even popular ones like Solana), which can break the spreadsheet approach.

Also, I’m not very comfortable with Google Sheets or Excel, and I prefer to write code.

All transactions done on a broker can be downloaded as CSV or XLSX files, with full details for each transaction (asset, volume, date, market price, fees, currencies, exchange rates, etc.).

So I decided to write a simple Rust program that reads transaction files downloaded from my brokers, performs the analysis, and writes the output to files.

To get market prices for my open positions, I built a scraper.

Pretty quickly, I had something better than my Google Sheets, and much more flexible for my needs.

Then, since I was between jobs for a few weeks, I decided to turn it into an app.

## Scope

The problem with side projects is that we’re usually full of energy when starting them, but we rarely finish. As a result, our GitHub accounts often look like cemeteries of unfinished projects that took hours or days to build and are ultimately useless.

This time, I decided to clearly define the scope of the project—what I would do and what I would not do—in order to build something that matched the time I had available and to know where to stop.

I wanted something usable for anyone facing the same problem I had, which requires a minimal but well-defined set of features.

### Market prices

For my personal use, market prices were not difficult to get.

I don’t have many open positions, so I built a simple scraper to fetch prices from websites like Google Finance or Yahoo Finance.

But providing market prices for any asset in a production app is a completely different challenge.

Even for my own use, I had to scrape multiple websites to get all the prices. This complexity would grow quickly as users import transactions involving very different assets. Scraping websites is also only viable at low volume.

That means relying on APIs, which are not free.

One option would be to avoid real-time prices and only provide the last closing price, refreshed nightly and stored in a cache.

But that could still lead to many unnecessary API calls. For example, users might have many open positions but only log in occasionally to check their data.

Fetching prices on demand and caching them could reduce API usage, but it introduces latency, which hurts the user experience.

Because of the variety of possible assets, I would likely need to regularly integrate new APIs to satisfy user needs.

And as the number of open positions and distinct assets grows, usage and rate-limit questions arise: should there be usage-based pricing? Hard limits?

And that’s only the visible part of the iceberg. It’s not necessarily difficult, but it’s a lot to manage.

In practice, open positions are already well detailed on broker platforms, since they’re part of the buying and selling process. Also, if you’re even moderately serious about trading, your open positions usually follow predefined rules: stop loss, risk/reward ratio, take profit, etc.

Once a transaction is made, you mostly let it run and only care about the final result—the realized P&L—when the position is closed.

For all these reasons, I decided to skip unrealized P&L in PNLParse.

Open positions are displayed in the app, but without market prices or unrealized P&L.

It could be a nice feature to have, of course, but it’s clearly out of scope here.

### Transaction files and supported brokers

Transactions can be downloaded from any broker with full details (asset, volume, date, market price, fees, currencies, exchange rates, etc.), which is very convenient.

- For users: whatever broker they use, trades can be imported into PNLParse.
- For me: I don’t need to rely on any external data source.

I wanted to start by supporting two brokers I personally use, so PNLParse includes native integrations for them, allowing me to import their transaction files directly.

Since it’s impossible to support every broker format in the world, a custom import format is essential. PNLParse provides a simple CSV format containing the essential information for each transaction.

The idea is to add native integrations for other brokers when requested by users. With the custom format, anyone can already use PNLParse.

### Minimal set of features

To make the app usable and valuable, I defined the following minimal feature set:

- Import trades from any broker (custom format + native integrations)
- Analysis of closed positions:
    - Realized P&L
    - Detailed breakdown of sold shares (essential to compute true realized P&L)
- Display open positions:
    - Without market prices and unrealized P&L
- Portfolio performance overview with a few key metrics

## Should it be a SaaS ?

Except at school, I’ve always built SaaS products. It feels like the default way of building apps today, and it does come with a lot of advantages.

But for this project, I wanted a limited scope and as little responsibility as possible once it was done.

Because of that, a SaaS didn’t make sense. I didn’t want to manage infrastructure, scaling, security, usage limits, authentication, or any of that.

Also, the core logic was already implemented in Rust, and I wanted to reuse it.

I had been wanting to try Tauri for a while — a Rust-based framework for building desktop applications — and it felt like a good fit for this project.

So PNLParse became an offline desktop app:

- All data comes from transaction files → it can work fully offline
- Users keep their data locally → no file uploads, no servers
- No security, auth, or usage-limit concerns → if users want to break their own app, that’s on them
- No infrastructure or scaling to manage → and no running costs for me

## Pricing

If PNLParse were a SaaS product, pricing would naturally be subscription-based, possibly with different plans depending on the volume of transactions.

But for an offline desktop app with a fixed feature set, that model doesn’t really make sense. Aside from adding new native broker imports, I don’t expect to spend much ongoing time on it. And adding more imports can attract new users anyway, so it’s something I’m happy to do for free.

A key difference with SaaS is enforcement. Since all data and logic live on the server, users must authenticate and have an active subscription, otherwise their requests are simply rejected. You’re guaranteed that users are paying.

With an offline desktop app, things are different. The data and logic live on the user’s machine. You can implement a licensing system, but it’s never fully secure.

A desktop app can always be hacked.

You can make hacking harder, but never bulletproof.

In the end, it comes down to a simple rule: **the cost of hacking versus the cost of buying**. If the price is reasonable for the value provided, most users won’t bother hacking it.

You *can* invest time to make it harder to crack, but that comes with additional complexity and responsibilities on my side—again, a tradeoff.

So my approach was simple: no license required. Anyone can download and use the app.

This gives users time to try it and see if it’s useful for them.

If it is, there’s a link to support the project. The amount is €35, which corresponds to what a lifetime license would have cost.

Why this approach?

When I was younger, I hacked a lot of software—like almost everyone else. When you’re young, you don’t have money, and piracy is common.

But once you do have money, you stop hacking. If a tool is useful, you just buy it and enjoy the better experience.

## Implementation

### Data storage

I initially stored the data in JSON files.

Setting up a database felt like overkill for PNLParse, and with `serde` it was easy to get something working quickly.

In practice, though, using SQLite is more efficient, even for a small app like this.

One reason is performance: deserialization in Rust can become relatively slow, while with SQLite the data is already typed and ready to query.

Another reason is flexibility. As I added filtering and ordering features to the app, SQL made it much simpler to express and execute those queries.

### My experience with using Tauri

I already had the backend working as a `clap`-based CLI to interact with it. I mainly needed to polish it to make it production-ready and suitable for use as an application backend.

For the frontend, I used Next.js and React, as I usually do.

Tauri essentially acts as the glue between the two and handles the build process. It’s surprisingly simple.

I added a second entry point to my Rust backend alongside the existing CLI, specifically for Tauri. I then defined a set of Tauri commands to interact with the backend, very similar to the CLI commands. It’s actually close to HTTP API controllers.

On the frontend side, instead of making HTTP calls to an API, I invoke Tauri commands directly.

With a bit of build configuration, everything was up and running.

Overall, the setup is very simple, and the result is a lightweight application with great performance.

## Feedback on building an offline desktop app

Building an offline desktop app with Tauri is actually very simple. I assume it’s fairly similar with alternatives like Electron.

It doesn’t feel more difficult or fundamentally different from building a web app with React.

The final installer is only about 7 MB, which makes the app very lightweight.

The biggest advantage for me is: no infrastructure, no security, and no authentication to manage.

With a licensing system, some of this would come back, but still far less than with a SaaS.

The biggest advantage for users is privacy. No data is shared, everything stays local, which is especially important for sensitive data like trading transactions.

Handling multiple platforms, however, is not as pleasant.

I need to build the app for Windows, macOS (both Apple Silicon and Intel), and various Linux distributions (Debian, Red Hat, etc.). On my Mac, I can only build macOS binaries. Fortunately, GitHub CI workflows make it possible to build for all platforms and attach the artifacts to a release. Testing on each platform, though, is a bit annoying.

That’s where web apps really shine: you build and test once, and it works everywhere.

Another downside is piracy. Even with a licensing system, a desktop app can be freely used and reverse-engineered. This simply doesn’t happen with SaaS products.

For Windows and macOS, applications should also be signed to provide a better user experience. That adds extra work, and signing macOS apps requires a $99/year Apple developer account.

---

If this had been a more serious, long-term project, with coming features like unrealized P&L for open positions, I would have gone with a SaaS.

But given the scope of PNLParse, an offline desktop app built with Tauri turned out to be the perfect choice.
