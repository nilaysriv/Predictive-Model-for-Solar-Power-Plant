# Solar Power Plant 10-Year Generation 

### 100MW Utility-Scale PV Simulation & Predictive Modeling

This project is a Python-based framework for simulating, visualizing, and predicting the performance of a 100MW Utility-Scale Solar PV plant. It combines physics-based modeling (NREL/Duffie-Beckman principles) with Machine Learning (Random Forest) to create a model capable of detecting performance variance.

## Key Features

* **Synthetic Data Generation:** Creates realistic hourly telemetry (8760 data points) for a Typical Meteorological Year (TMY), including:
    * **Seasonality & Daily Bell Curves.**
    * **Stochastic Cloud Cover.**
    * **Realistic Noise:** Simulates sensor error and efficiency losses (soiling/wiring) to mimic real-world telemetry (R² < 1.0).
* **Physics Visualization:**
    * Demonstrates **Inverter Clipping** (DC Capacity > AC Limit).
    * Visualizes seasonal trends and daily irradiance profiles.
* **Machine Learning :**
    * Trains a **Random Forest Regressor** to learn the plant's physics.
    * Validates model performance against "noisy" data.
    * Visualizes Residuals (Variance) to identify performance gaps.
* **Financial Forecasting:**
    * Extrapolates 10-year generation tables.
    * Calculates **P50 (Base), P70 (Conservative), and P90 (Bankable)** generation scenarios with annual degradation.

## Software Used

* **Python 3.x**
* **Pandas:** Data manipulation and time-series analysis.
* **NumPy:** Vectorized physics calculations.
* **Scikit-Learn:** Random Forest implementation for the Digital Twin.
* **Matplotlib & Seaborn:** Visualization dashboards.

## Installation

Ensure you have the required libraries installed:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jinja2

```

## Project Structure

The project is designed as a linear pipeline within a Jupyter Notebook:

1. **Data Generation:** Physics engine creates the dataset with embedded noise.
2. **Visualization:** Plots raw telemetry to confirm physical phenomena.
3. **Model Training:** Splits data (80/20), trains the Random Forest based on the visualized data, and calculates R²/MAE.
4. **Validation:** Plots Actual vs. Predicted values and a Residual Bar Chart to show model variance.
5. **Forecasting:** Generates the 10-Year Annual Generation Table (P50/P70/P90).

##  Methodology

### 1. The Physics Model

The simulation assumes a **1.3 DC:AC Ratio** (130MW DC / 100MW AC).

* **Clipping:** The model strictly enforces a 100MW limit. Any DC generation above this is "clipped" (energy loss), which is a non-linear behavior critical for the ML model to learn.
* **Variance:** Random noise is injected into the "Efficiency" and "Sensor" variables to simulate real-world data imperfections, ensuring the ML model does not achieve an artificial 100% accuracy.

### 2. Machine Learning Choice

**Selected Model:** Random Forest Regressor
**Why?** Solar generation is non-linear due to:

1. **Inverter Clipping:** The relationship between Irradiance and Output flattens abruptly at 100MW. Linear Regression cannot model this "flat top."
2. **Temperature Coefficients:** Efficiency drops as heat increases, creating a complex interaction between GHI (Sunlight) and Temperature.
Random Forest captures these decision boundaries naturally.

## Example Output

The notebook produces a 10-year forecast table similar to:

| Year | P50 Generation (MWh) | P70 Generation (MWh) | P90 Generation (MWh) |
| --- | --- | --- | --- |
| **1** | 175,200.00 | 166,440.00 | 157,680.00 |
| **2** | 174,324.00 | 165,607.80 | 156,891.60 |
| ... | ... | ... | ... |
| **10** | 167,472.00 | 159,098.40 | 150,724.80 |

