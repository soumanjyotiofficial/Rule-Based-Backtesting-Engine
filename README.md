# 📈 Rule Based Backtesting Engine (Python)

A rule-based backtesting engine for systematic trading strategies with realistic Indian equity transaction cost modeling (Zerodha brokerage structure).

This engine supports:

- Long & Short trades  
- Stop-loss & Target-based exits  
- Risk-based position sizing  
- Detailed transaction cost breakdown  
- Trade ledger generation  

---

## 🚀 Features

### 1️⃣ Capital & Risk Management

- Configurable initial capital
- Fixed % risk per trade (default: 1%)
- Dynamic position sizing

Position Size Formula:

Position Size = (Capital × Risk %) / |Entry Price − Stop Loss|

---

### 2️⃣ Transaction Cost Model (Indian Equity)

Charges modeled as per Zerodha structure:

Includes:
- SEBI Charges
- Transaction Charges
- Brokerage (capped at ₹30)
- GST (18%)
- STT
- Stamp Duty (Buy side only)

Transaction cost function returns:

[SEBI, Transaction Cost, Brokerage, GST, STT, Stamp Duty, Total Cost]

---

### 3️⃣ Strategy Logic

Signal Rules:

- signal = 1 → Long Entry
- signal = -1 → Short Entry

Default Risk Parameters:

- Stop Loss: 2%
- Target: 4%
- Risk per trade: 1% of capital

Exit Conditions:

- Stop Loss Hit
- Target Hit

---

## 🧠 Engine Architecture

Core Class:

class Backtesting_Engine

Key Methods:

- __brokerages__() → Initializes brokerage structure  
- transaction_cost_caluclation() → Computes full trade charges  
- simulator() → Executes strategy row by row  
- ledger → Stores trade history  

---

## 📊 Required Input Data

The engine expects a Pandas DataFrame with:

| Column  | Description |
|----------|-------------|
| Date     | Trade date |
| Symbol   | Stock ticker |
| Close    | Closing price |
| signal   | Trading signal (1 or -1) |

Example:

Date        Symbol    Close    signal  
2023-01-01  RELIANCE  2500     1  

---

## ⚙️ How To Use

### 1️⃣ Prepare Data

consolidated_data = consolidate_data(indicator_data)

---

### 2️⃣ Initialize Engine

BE = Backtesting_Engine(
    data=consolidated_data,
    initial_capital=1000000,
    lot_size=1,
    sl=0.02,
    tgt=0.04
)

---

### 3️⃣ Run Backtest

ledger = BE.simulator()

---

## 📒 Ledger Structure

| Column | Description |
|--------|------------|
| Date | Execution date |
| Symbol | Stock |
| Price | Execution price |
| Qty | Quantity traded |
| SL | Stop Loss |
| TGT | Target |
| Type | Buy / Sell |
| Transaction Cost | Cost breakdown array |
| Amount | Cash flow impact |
| Y | Entry / SL / TGT |

---

## 💰 Capital Flow Logic

Long Entry:
Capital -= (Price × Qty + Cost)

Long Exit:
Capital += (Price × Qty − Cost)

Short Entry:
Capital += (Price × Qty − Cost)

Short Exit:
Capital -= (Price × Qty + Cost)

---

## 📦 Dependencies

pip install pandas numpy

---

## 🔮 Future Improvements

- Equity curve tracking
- Drawdown calculation
- Sharpe ratio
- CAGR
- Slippage modeling
- Portfolio-level exposure control
- Performance analytics module

---

## 📌 Limitations

- Only SL/TGT exits implemented
- No trailing stop
- No partial position scaling
- Single position per symbol
- No portfolio heat management

---

## 🧪 Example: Transaction Cost Calculation

list(BE.transaction_cost_caluclation(1000, 'buy', 100))

Returns:

[SEBI, TC, Brokerage, GST, STT, Stamp Duty, Total Cost]

---

## 👨‍💻 Author

Souman Jyoti  
Quantitative Finance & Python Enthusiast  

---

This project is ideal for:

- Quant research practice
- Strategy validation
- Risk management experimentation
- Learning realistic trade accounting
- Indian market simulation
