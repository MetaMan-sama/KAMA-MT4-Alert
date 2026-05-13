# Adaptive Moving Average Alert (AMA / KAMA) — MQL4 Script

A MetaTrader 4 script that continuously monitors price action using the **Kaufman Adaptive Moving Average (KAMA/AMA)** and fires alerts whenever a bullish or bearish trend shift is detected.

---

## Overview

The Adaptive Moving Average (AMA), also known as the Kaufman Adaptive Moving Average (KAMA), adjusts its smoothing speed based on market efficiency. It moves quickly during trending conditions and slows down during choppy, sideways markets — making it a reliable filter for distinguishing real trend changes from noise.

This script runs in a loop inside MetaTrader 4, recalculates the AMA every minute, and notifies you via sound alert, email, or push notification whenever the AMA direction changes.

---

## Features

- **Adaptive trend detection** using the Kaufman Efficiency Ratio (ER)
- **Configurable symbol and timeframe** — works on any MT4 instrument
- **Three notification channels:** sound alert, email, and mobile push
- **Lightweight loop** — polls once per minute (`Sleep(60000)`)
- Prints all signals to the MT4 **Experts log** for easy review

---

## How It Works

1. **Efficiency Ratio (ER)** is calculated as the ratio of net price change over the lookback period to the sum of individual bar-to-bar price changes (total path). A high ER means a strong trend; a low ER means choppiness.
2. **Smoothing Constant (SC)** blends between a fast EMA factor and a slow EMA factor, weighted by the ER.
3. **AMA** is updated each tick using: `AMA = AMA + SC² × (Price − AMA)`
4. On each iteration, the new AMA is compared to the previous value:
   - `AMA rising` → **Bullish Trend Detected**
   - `AMA falling` → **Bearish Trend Detected**

---

## Input Parameters

| Parameter      | Type               | Default    | Description                              |
|----------------|--------------------|------------|------------------------------------------|
| `TradeSymbol`  | string             | `EURUSD`   | The instrument to monitor                |
| `Timeframe`    | ENUM_TIMEFRAMES    | `PERIOD_H1`| Chart timeframe for AMA calculation      |
| `Period`       | int                | `10`       | Lookback period for the Efficiency Ratio |
| `FastEMA`      | double             | `2.0`      | Fast EMA smoothing factor                |
| `SlowEMA`      | double             | `30.0`     | Slow EMA smoothing factor                |
| `EnableAlerts` | bool               | `true`     | Fire an on-screen/sound alert            |
| `EnableEmail`  | bool               | `false`    | Send an email notification               |
| `EnablePush`   | bool               | `false`    | Send a mobile push notification          |

---

## Installation

1. Copy `Adaptive_Moving_Average__AMA_KAMA__001.mq4` into your MetaTrader 4 **Scripts** folder:
   ```
   MQL4/Scripts/
   ```
2. Open MetaEditor and **compile** the file (F7), or let MT4 compile it automatically on restart.
3. In MT4, open the **Navigator** panel → **Scripts** → drag the script onto any chart.
4. Configure the input parameters in the dialog that appears and click **OK**.

> **Note:** For email and push notifications to work, configure the corresponding settings under **Tools → Options → Email / Notifications** in MetaTrader 4.

---

## Usage Notes

- This is a **script** (not an indicator or EA), so it runs once per chart attach and loops internally using `Sleep()`. It will stop if you remove it from the chart or call `OnStop()`.
- The AMA value is initialized from the current close price on first run, so the first comparison is skipped (`PrevAMA == 0` guard).
- The `static` AMA accumulator inside `CalculateAMA()` persists across loop iterations but resets if the script is restarted.
- Increasing `Period` makes the ER calculation smoother and less reactive to short-term noise.
- Lowering `FastEMA` (e.g. to `2`) and raising `SlowEMA` (e.g. to `30`) widens the gap between fast and slow tracking, giving the AMA more adaptability.

---

## Alert Message Format

```
Bullish Trend Detected on EURUSD (Timeframe: PERIOD_H1)
AMA Value: 1.08452
```

---

## Requirements

- MetaTrader 4 (any build supporting `#property strict`)
- MQL4 compiler (included with MT4 / MetaEditor)

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
