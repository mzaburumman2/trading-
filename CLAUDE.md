# Kronos candle-forecasting research project

## Rules (non-negotiable)
- Download code only from github.com/shiyu-coder/Kronos and models only from the NeoQuasar organisation on Hugging Face.
- Use only public, read-only market data.
- Never ask for, store or use API keys, wallet keys or passwords.
- Never write code that places a real order.
- Do not invent results. If a step fails, show the error and explain it.

## How to work
- One phase at a time. After each phase, explain in plain English what was done and what the output means, then wait for the user to say "continue".
- The user is a beginner with the terminal but understands finance.
- Read the Kronos repo's code before relying on its API; report anything that differs from the brief.

## Phases
1. Install: venv (Python 3.10+), clone Kronos into ./Kronos, install requirements.txt, report device (CUDA / mps / CPU).
2. First forecast: forecast.py — last 400 hourly BTC/USDT candles via ccxt (no keys), Kronos-small + Kronos-Tokenizer-base, lookback <= 512, forecast 24 candles -> forecast.csv + forecast.png. CLI options: symbol, timeframe, exchange.
3. Uncertainty band: 30 sample paths; save all paths + mean/min/max per candle; plot mean + min-max band; print final band width as % of current price. The band shows sample disagreement, NOT a confidence interval.
4. Backtest: backtest.py — walk-forward over last 6 months, no lookahead, 400-candle context, 24-candle horizon. Estimate runtime and pick a practical step size; resumable. Metrics: direction accuracy, mean close error %, paper P&L after 0.1% fees/trade, max drawdown, worst losing streak. Benchmarks: naive (last close, error only), buy & hold (P&L), random long/short with same fees (direction + P&L). Side-by-side table, equity curve chart, second 6-month period, and ETH/USDT. Honest verdict incl. noise.
5. TradingView overlay: Pine Script v5 indicator from forecast.csv with hardcoded values; mean line + shaded min-max band from last real candle, one bar per step; title "Kronos forecast (research)". Remind: same exchange & timeframe.
6. Scorecard: go/no-go — beats benchmarks after fees in >= 2 periods; holds on an untuned symbol; several weeks of logged paper trading (still to do if not done). Any failure = no-go. Do not soften.
