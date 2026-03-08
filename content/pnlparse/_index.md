+++
title = "PNLParse"
description = "Analyze trades and returns from broker files. Your data never leaves your machine."
+++

*[How I built PNLParse](/content/software-engineering/building-pnlparse.md)*

Tracking your trades across multiple brokers is frustrating—and spreadsheets are a nightmare to maintain. PNLParse makes it effortless.

Brokers make it easy to trade—but not to understand your results. If you use multiple brokers, tracking everything manually becomes nearly impossible.

## What is PNLParse?

PNLParse reads the transaction files you download from your brokers and provides detailed insights on your trades, along with portfolio performance metrics.

No spreadsheets. No cloud uploads. Just clean, private analytics on your own computer.

<img src="/pnlparse/app.png" alt="PNLParse Screenshot" />

## Privacy First

Most analytics tools are SaaS, which means your trading data leaves your computer.

PNLParse runs **entirely offline**. Your trades **never leave your machine**.

## Download

PNLParse is available for Windows, Linux, and macOS.

[Releases](https://github.com/antoineprdhmm/pnlparse/releases)

### MacOS users:

> Since PNLParse is not yet registered on the Apple Store, you may see a security warning when launching the app. To bypass it:
> 1. Open **System Settings** → **Privacy & Security**
> 2. Scroll to find **PNLParse** was blocked from use
> 3. Click **Open Anyway** and launch the app again

> If you are on Apple Silicon and see **"PNLParse" is damaged and can't be opened**, use the Intel version instead.

<img src="/pnlparse/macos_secu.png" width="400" alt="macOS Security Settings" />

## Supported Brokers

PNLParse can import transactions from **any broker** via a custom CSV format.

Some brokers are **natively supported** for simplicity. If yours isn't, feel free to [create a Pull Request](https://github.com/antoineprdhmm/pnlparse).

---

PNLParse makes trading analytics **simple, fast, and private** so you can focus on trading, not spreadsheets.
