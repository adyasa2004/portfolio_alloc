# 📊 AI-Powered Portfolio Advisor

An end-to-end intelligent portfolio advisory system that combines **classical quantitative finance** with **machine learning** and **agentic AI** to deliver personalized investment recommendations through natural language conversation.

## 🏗️ Architecture

```
User Query (natural language)
       │
       ▼
┌──────────────┐
│  Gemini LLM  │ ◄── System prompt + tool definitions
│   (Agent)    │
└──────┬───────┘
       │ Tool calls
       ▼
┌──────────────────────────────────────────────────┐
│              Tool Implementations                 │
│                                                   │
│  ┌─────────────┐  ┌──────────────┐               │
│  │ Data Fetcher │  │Risk Analysis │               │
│  │  (yfinance)  │  │  (Sharpe,    │               │
│  │              │  │   Cov, etc.) │               │
│  └─────────────┘  └──────────────┘               │
│                                                   │
│  ┌─────────────┐  ┌──────────────┐               │
│  │  Optimizer   │  │ML Forecaster │               │
│  │ (Markowitz   │  │(RandomForest │               │
│  │  CVXPY)      │  │  sklearn)    │               │
│  └─────────────┘  └──────────────┘               │
└──────────────────────────────────────────────────┘
       │
       ▼
  Natural language response + data
```

## ✨ Features

| Feature | Description |
|---------|-------------|
| **Conversational Interface** | Ask questions in natural language via Streamlit chat |
| **Markowitz Optimization** | Constrained mean-variance optimization with sector/position limits |
| **ML Return Forecasting** | Random Forest model with walk-forward validation |
| **Risk Analytics** | Sharpe ratios, volatility, correlation analysis |
| **Tool-Calling Agent** | Gemini LLM autonomously selects and chains analytical tools |
| **Live Market Data** | Real-time price fetching via yfinance |

## 🚀 Quick Start

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Get a Gemini API Key
Get one free at [Google AI Studio](https://aistudio.google.com/apikey)

### 3. Run the App
```bash
streamlit run app.py
```

Enter your API key in the sidebar, then start chatting!

## 💬 Example Queries

| Query | What the Agent Does |
|-------|-------------------|
| *"Build me a conservative portfolio"* | Fetches data → Runs optimization (low risk) → Explains allocation |
| *"Compare NVDA vs AAPL risk profiles"* | Fetches data → Computes risk metrics → Compares side-by-side |
| *"Forecast next-month returns"* | Builds features → Trains RF model → Returns predictions + metrics |
| *"Optimize with high risk tolerance"* | Runs optimization (high risk / low λ) → Shows aggressive allocation |

## 📁 Project Structure

```
Portfolio-Allocation/
├── app.py                    # Streamlit chat interface
├── requirements.txt          # Python dependencies
├── README.md
├── src/
│   ├── __init__.py
│   ├── config.py             # Stock universe, sectors, defaults
│   ├── data_fetcher.py       # yfinance data collection
│   ├── risk_analysis.py      # Returns, covariance, Sharpe ratios
│   ├── optimizer.py          # Markowitz mean-variance optimization
│   ├── ml_forecaster.py      # Random Forest return predictions
│   └── agent.py              # Gemini AI agent with tool-calling
└── notebooks/
    ├── optimising portfolio.ipynb       # Original exploration
    └── optimising portfolio copy.ipynb  # Refined analysis
```

## 🔧 Tech Stack

- **Python** — Core language
- **yfinance** — Market data
- **CVXPY** — Convex optimization
- **scikit-learn** — Random Forest ML model
- **Google Gemini API** — LLM with tool-calling
- **Streamlit** — Chat UI frontend

## 📊 Stock Universe

18 US large-cap stocks across 6 sectors:

| Sector | Stocks |
|--------|--------|
| Technology | AAPL, MSFT, NVDA |
| Finance | JPM, BAC, GS |
| Energy | XOM, CVX, COP |
| Consumer Staples | PG, KO, WMT |
| Healthcare | JNJ, UNH, PFE |
| Industrial | CAT, BA, MMM |

## 📝 Methodology

### Optimization
- **Framework**: Markowitz mean-variance with convex constraints
- **Constraints**: Long-only, max 10% per stock, max 25% per sector
- **Risk tolerance**: Mapped to risk-aversion parameter λ (low=6, medium=3, high=1)

### ML Forecasting
- **Model**: Random Forest Regressor (300 trees, max depth 6)
- **Features**: 1-month lag, 3/6-month momentum, 3/6-month volatility, market return
- **Validation**: Walk-forward (train ≤2022, test 2023-2025)
- **Metric**: Directional accuracy ~53%, outperforms zero-return baseline
