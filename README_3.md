# Zambia Exchange Rate Forecasting

Probabilistic forecasting of the Zambian Kwacha (ZMW/USD), 2010–2024, using a trend–volatility decomposition and quantile regression — and a direct test of whether copper prices or foreign exchange reserves actually drive the currency.

## What this project does

Most commentary on the Kwacha treats copper prices as the main driver. This project tests that claim against 15 years of monthly data using a feature-importance analysis across six macroeconomic variables, then builds a probabilistic forecasting model — one that reports a range of plausible outcomes rather than a single guessed number — and is explicit about where that model succeeds and where it doesn't.

**Key finding:** foreign exchange reserves, not copper prices, are the strongest single driver of Kwacha movements in this data — accounting for roughly half the explained variation in a multivariate analysis.

**Key limitation:** the model's forecast intervals remain miscalibrated specifically on the upside, particularly during the 2022–2024 period of historically unprecedented depreciation. Two targeted fixes were tested; neither fully closed the gap. This is treated as a finding in itself, not swept under the rug.

## Methodology

1. **Data collection** — exchange rate (Bank of Zambia), inflation (Central Statistical Office Zambia), FX reserves (IMF), copper production and electricity generation (Zambia Data Portal), and copper price / US Fed Funds rate / oil price (FRED).
2. **Exploratory analysis** — distributional properties of the exchange rate, a live side-by-side test of a misleading candidate variable (electricity generation) before excluding it, and a Random Forest feature-importance ranking.
3. **Trend–volatility decomposition** — the exchange rate is split into a trend component and a time-varying volatility component, rather than treated as having one constant level of risk throughout.
4. **Probabilistic forecasting** — Gradient Boosting quantile regression produces 10th/50th/90th percentile forecasts, evaluated with pinball loss and empirical coverage rather than a single point-accuracy metric.
5. **Model comparison and robustness checks** — a GARCH(1,1) volatility specification is compared directly against a simpler realized-volatility estimator, and two further refinements are tested against the residual miscalibration, all computed live in the notebook rather than asserted from results elsewhere.

## Repository structure

```
zambia-exchange-rate-forecasting/
├── zambia_kwacha_trend_volatility_forecasting.ipynb   # main analysis notebook
├── data/
│   ├── zmw_usd_monthly_clean.csv                      # cleaned monthly exchange rate
│   ├── zambia_monthly_inflation_raw.csv                # monthly CPI inflation
│   ├── zambia_fx_reserves_monthly.csv                  # FX reserves excl. gold
│   ├── zambia_copper_electricity_annual.csv            # annual production series
│   └── AVERAGE_FXRATES_10.xlsx                         # raw Bank of Zambia daily FX rates
│                                                          (source file for zmw_usd_monthly_clean.csv;
│                                                           not read directly by the notebook, included
│                                                           for provenance and independent verification)
├── requirements.txt
└── README.md
```

## Running this notebook

**1. Clone the repository**

```bash
git clone https://github.com/BoldwinMax/zambia-exchange-rate-forecasting.git
cd zambia-exchange-rate-forecasting
```

**2. Install dependencies**

```bash
pip install -r requirements.txt
```

**3. Get a free FRED API key**

Three series (copper price, US Federal Funds Rate, Brent crude oil) are fetched live from the Federal Reserve Bank of St. Louis. Get a free key at [fred.stlouisfed.org/docs/api/api_key.html](https://fred.stlouisfed.org/docs/api/api_key.html), then set it as an environment variable:

```bash
# macOS / Linux
export FRED_API_KEY='your_key_here'

# Windows (PowerShell)
$env:FRED_API_KEY='your_key_here'
```

**4. Launch the notebook**

```bash
jupyter notebook zambia_kwacha_trend_volatility_forecasting.ipynb
```

Run the cells top to bottom. The notebook is self-contained — no external file dependencies beyond what's in this repository.

## Data sources

| Series | Source |
|---|---|
| Exchange rate (ZMW/USD) | Bank of Zambia |
| Monthly inflation | Central Statistical Office Zambia, via [Zambia Data Portal](https://zambia.opendataforafrica.org) |
| FX reserves | IMF International Liquidity dataset |
| Copper production, electricity generation | Zambia Data Portal |
| Copper price, Fed Funds Rate, oil price | [FRED](https://fred.stlouisfed.org), Federal Reserve Bank of St. Louis |

Full provenance and access details are in the notebook's References section.

## Author

Boldwin Mweemba
[LinkedIn](https://www.linkedin.com/in/boldwin-mweemba) · [GitHub](https://github.com/BoldwinMax)

## License

This project is shared for educational and research purposes. Feel free to fork, adapt, and build on it — attribution appreciated.
