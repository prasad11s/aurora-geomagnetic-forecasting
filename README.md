# Multi-Horizon Geomagnetic Kp Index Prediction Using LSTM

Predicting geomagnetic storm intensity (Kp index) at 1, 3, 6, and 12-hour horizons simultaneously using a single LSTM neural network trained on NASA OMNI2 solar wind data. Achieves **48% RMSE improvement** over baseline models for 1-hour forecasting.

![Project Poster](aurora_poster.png)

---

## Why This Matters

Geomagnetic storms caused by solar wind disturbances can disrupt satellite operations, damage power grids, interfere with GPS navigation, and degrade radio communications. The economic impact of severe storms runs into billions of dollars. Accurate Kp prediction enables satellite operators, power grid managers, and aurora enthusiasts to prepare in advance.

---

## Key Results

| Forecast Horizon | RMSE | Improvement vs Baseline |
|---|---|---|
| 1-hour | 0.691 Kp | 48.2% |
| 3-hour | 0.894 Kp | 33.0% |
| 6-hour | 1.067 Kp | 20.0% |
| 12-hour | 1.211 Kp | 9.2% |

**Additional metrics (1-hour ahead):**
- MAE: 0.489 Kp units
- R²: 0.727 (explains 73% of variance)
- 89.1% of predictions within ±1.0 Kp of actual value

---

## Tech Stack

| Component | Tool |
|---|---|
| Language | Python |
| Deep Learning | TensorFlow / Keras |
| Data Processing | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Data Source | NASA OMNI2 Database |

---

## Dataset

**Source:** NASA OMNI2 High Resolution Database  
**Coverage:** January 2013 to December 2024 (11 years)  
**Size:** 96,552 hourly measurements  
**Features:** 55 solar wind and geomagnetic parameters, reduced to 15 via correlation analysis

**Train / Validation / Test Split (chronological):**
- Training: 2013 to 2018
- Validation: 2019 to 2020
- Testing: 2021 to 2024

---

## Model Architecture

- **Type:** Multi-output LSTM neural network
- **Input:** 12-hour lookback sequences (12 timesteps × 15 features)
- **Output:** 4 simultaneous predictions [Kp_1h, Kp_3h, Kp_6h, Kp_12h]
- **Regularization:** 50% dropout + L2 regularization + early stopping
- **Optimizer:** Adam (lr=0.001)
- **Batch size:** 64

---

## Project Structure

```
aurora-geomagnetic-forecasting/
│
├── kp_prediction_notebook.ipynb       # Complete pipeline: data prep, EDA, modeling, evaluation
├── aurora_poster.png                  # Conference poster presentation
├── Aurora_Kp_Prediction_Report_V2.0.docx  # Full project report
│
├── data/
│   └── omni2_combined.csv             # Processed NASA OMNI2 dataset
│
├── figures/                           # All EDA and results plots
│   └── *.png
│
└── models/
    ├── best_model.keras               # Best saved LSTM model
    ├── final_lstm_model.keras         # Final trained model
    ├── scaler_X.pkl                   # Feature scaler
    ├── scaler_y.pkl                   # Target scaler
    ├── training_history.pkl           # Loss curves per epoch
    └── results_summary.pkl            # RMSE/MAE scores
```

---

## How to Run

```bash
git clone https://github.com/prasad11s/aurora-geomagnetic-forecasting.git
cd aurora-geomagnetic-forecasting
pip install tensorflow pandas numpy matplotlib seaborn scikit-learn
```

Open `kp_prediction_notebook.ipynb` in Jupyter and run all cells. The data file is included in the `data/` folder.

---

## Key Challenges Solved

**Overfitting:** Initial model had training loss of 0.232 vs validation loss of 0.818. Fixed by progressively adding 50% dropout, L2 regularization, and early stopping, bringing final validation loss to 0.574.

**Data leakage:** Top correlated features (ap, AE, AL, AU) are geomagnetic indices derived from the same ground measurements as Kp. Validated that using 24-hour historical lookback with proper temporal gaps prevents any leakage.

**Column mapping errors:** NASA OMNI2 format had incorrect initial column assignments. Manually cross-referenced official NASA documentation to correctly identify all 55 parameters including Kp at column 38.

---

## Authors

**Prasad Shimpi** — MS Applied Data Science, Syracuse University  
**Tanay Pratap Singh** — MS Applied Data Science, Syracuse University  
**Nikita Sharma** — MS Applied Data Science, Syracuse University

[LinkedIn](https://linkedin.com/in/prasadshimpi) | [GitHub](https://github.com/prasad11s)
