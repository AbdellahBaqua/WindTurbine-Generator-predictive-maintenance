Direct Forecast Dashboard Documentation
=======================================

.. currentmodule:: app

Overview
--------
The Direct Forecast Dashboard provides real-time monitoring and forecasting capabilities for wind turbine generators.

Key Features
-----------
* Real-time data monitoring
* Advanced forecasting
* Interactive visualizations
* Predictive maintenance alerts

Functions
---------

.. _Dashboard_App_load_data:

``load_data(uploaded_file, timestamp_col="time")``
   Loads data from an uploaded CSV file, optionally parsing a timestamp column.

   :param uploaded_file: The file-like object from Streamlit's file_uploader.
   :type uploaded_file: file
   :param timestamp_col: The name of the timestamp column in the CSV. Defaults to "time".
   :type timestamp_col: str, optional
   :returns: Loaded data as a pandas DataFrame, or None if an error occurs.
   :rtype: pandas.DataFrame or None

.. _Dashboard_App_preprocess_data:

``preprocess_data()``
   (Add your description here)

   (Add parameters, returns, and raises if applicable)

.. _Dashboard_App_load_model:

``load_model()``
   (Add your description here)

   (Add parameters, returns, and raises if applicable)

.. _Dashboard_App_predict:

``predict()``
   (Add your description here)

   (Add parameters, returns, and raises if applicable)

.. _Dashboard_App_inverse_transform:

``inverse_transform()``
   (Add your description here)

   (Add parameters, returns, and raises if applicable)

.. _Dashboard_App_plot_forecast:

``plot_forecast()``
   (Add your description here)

   (Add parameters, returns, and raises if applicable)

Code Structure
-------------
The application is organized into several key components:

* Data Loading and Preprocessing
* Model Management
* Prediction Pipeline
* Visualization Tools

Error Handling
-------------
The application includes comprehensive error handling for:

* Data loading and validation
* Model loading and prediction
* Visualization generation

Key Features
------------

Code Structure
--------------

Error Handling
--------------
.. toctree::
   :maxdepth: 2
   :caption: Related Documentation:
   
   installation.rst
   usage.rst
   preprocessing.rst
   modelstesting.rst
   timeforecasting.rst
   
   modules.rst

For installation instructions, see :doc:`installation`. For usage examples, refer to :doc:`usage`.

Key Features
-----------

* **Two Modes**:
    * Live RUL Prediction (see :doc:`timeforecasting`)
    * Component Health Forecasting
* **Interactive Visualization**: Uses Plotly for dynamic, zoomable charts.
* **Scalable Input Handling**: Accepts CSV files with historical sensor data (details in :doc:`preprocessing`).
* **Caching for Performance**: Optimizes model loading and data processing.

Code Structure
-------------

1. **App Configuration** (``st.set_page_config``)
    * Sets up the dashboard layout with a title ("Direct Forecast Dashboard") and an icon (⚡).

2. **Model & Configuration** 
    * See model architecture details in :doc:`modules`
    * Testing results available in :doc:`modetesting1`

3. **Cached Helper Functions**
    * Implementation details in :doc:`utils`

5. **Streamlit UI**
    * **Sidebar Controls**
        * Page Selection: Toggles between RUL Prediction and Component Health Forecasting.
        * File Uploader: Accepts CSV files with historical data.
        * Forecast Starting Point: Slider to select where the forecast begins.

How to Use
----------

See comprehensive usage guide in :doc:`usage`.

Dependencies
------------

* **Python Libraries**:
    * Full list in :doc:`requirements.txt <requirements>`
* **Model Files**:
    * ``model_BiLSTM.pth`` (trained PyTorch model)
    * ``main_scaler.joblib`` (feature scaler)

Error Handling
-------------

* For troubleshooting, see :doc:`usage` and :doc:`utils`.