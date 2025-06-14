Direct Forecast Dashboard Documentation
=====================================

Overview
--------

This Streamlit-based dashboard provides predictive maintenance capabilities using a Bidirectional LSTM (BiLSTM) deep learning model. It forecasts:

* Remaining Useful Life (RUL) of a component by predicting when degradation will cross a failure threshold.
* Multi-variable forecasts (degradation, temperature, maintenance time) for component health monitoring.

Key Features
-----------

* **Two Modes**:
    * Live RUL Prediction: Estimates how many cycles remain before a component fails.
    * Component Health Forecasting: Projects future trends of degradation, temperature, and maintenance time.
* **Interactive Visualization**: Uses Plotly for dynamic, zoomable charts.
* **Scalable Input Handling**: Accepts CSV files with historical sensor data.
* **Caching for Performance**: Optimizes model loading and data processing.

Code Structure
-------------

1. **App Configuration** (``st.set_page_config``)
    * Sets up the dashboard layout with a title ("Direct Forecast Dashboard") and an icon (⚡).

2. **Model & Configuration** (``MODEL_CONFIG`` & ``BiLSTMModel``)
    * ``MODEL_CONFIG`` defines:
        * ``window_size``: How much historical data the model uses for predictions.
        * ``future_steps``: How far into the future it forecasts.
        * ``hidden_size``, ``num_layers``, ``dropout``: Neural network hyperparameters.
        * ``target_columns``: Features to predict (degradation, temperature_estimated, time_since_last_maintenance).
        * ``degradation_threshold``: Failure threshold (0.66 = 66% degradation).
    * ``BiLSTMModel`` is a PyTorch neural network with:
        * A bidirectional LSTM layer for sequence learning.
        * A fully connected (linear) layer to produce predictions.

3. **Cached Helper Functions**
    * ``load_model_and_scaler()``
        * **Purpose**: Loads the trained BiLSTM model and the scaler (for normalization).
        * **Caching**: Uses ``@st.cache_resource`` to avoid reloading on every interaction.
        * **Checks**: Ensures model and scaler files exist before loading.
    * ``load_data()``
        * **Purpose**: Reads uploaded CSV files into a Pandas DataFrame.
        * **Caching**: Uses ``@st.cache_data`` to avoid reprocessing the same file.

4. **Core Forecasting Function** (``run_direct_forecast``)
    * **Input**: A history window (last ``window_size`` steps).
    * **Steps**:
        1. Normalizes the input using the pre-fitted scaler.
        2. Runs the model to generate future predictions.
        3. Reshapes predictions into a structured DataFrame.
        4. Inverse-transforms to original scale.
    * **Output**: A DataFrame with forecasts for ``target_columns``.

5. **Streamlit UI**
    * **Sidebar Controls**
        * Page Selection: Toggles between RUL Prediction and Component Health Forecasting.
        * File Uploader: Accepts CSV files with historical data.
        * Forecast Starting Point: Slider to select where the forecast begins.
    * **Live RUL Prediction**
        * **How it works**:
            1. Forecasts degradation until it crosses the threshold.
            2. Calculates RUL as the number of steps until failure.
            3. Plots historical vs. forecasted degradation with failure point.
        * **Output**:
            * RUL Value (in cycles).
            * Interactive Plot showing degradation trend and threshold.
    * **Component Health Forecasting**
        * **How it works**:
            1. Forecasts all target variables (degradation, temperature_estimated, time_since_last_maintenance).
            2. Allows selecting which variables to display.
        * **Output**:
            * Forecast Preview Table (first few rows).
            * Interactive Plots for each selected variable.

How to Use
----------

1. **Upload Data**:
    * Provide a CSV with historical sensor data (must include ``target_columns``).
2. **Select Forecast Point**:
    * Adjust the slider to choose where forecasting starts.
3. **Run Prediction**:
    * Click "Calculate RUL" or "Forecast Components".
4. **Interpret Results**:
    * RUL Mode: See remaining cycles before failure.
    * Health Forecast Mode: Compare historical vs. predicted trends.

Dependencies
------------

* **Python Libraries**:
    * ``streamlit`` (UI)
    * ``pandas``, ``numpy`` (data handling)
    * ``torch`` (PyTorch for deep learning)
    * ``joblib`` (model/scaler loading)
    * ``plotly`` (interactive plots)
* **Model Files**:
    * ``model_BiLSTM.pth`` (trained PyTorch model)
    * ``main_scaler.joblib`` (feature scaler)

Error Handling
-------------

* **Missing Files**: Shows an error if model/scaler files are missing.
* **Invalid Data**: Alerts if the CSV is corrupted or lacks required columns.
* **Forecast Limits**: If degradation doesn't reach the threshold, warns that RUL exceeds forecast window.