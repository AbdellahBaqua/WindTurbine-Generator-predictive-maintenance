Models Testing Documentation
============================

Overview
--------

This notebook tests four different neural network models for a single output prediction task:

* BiLSTM
* BiGRU
* Conv1D_LSTM
* LSTM_Attention

Model Training Process
---------------------

Each model was trained with:

* Early stopping to prevent overfitting
* 500 maximum epochs
* Training and validation loss tracking
* Best model saved (``.pth`` files)
* Scaler saved (``.joblib`` files)

Performance Comparison
---------------------

.. list-table:: Model Performance Metrics
   :widths: 30 20 20
   :header-rows: 1

   * - Model
     - R² Score
     - MSE
   * - BiLSTM
     - 0.991752
     - 0.000339
   * - BiGRU
     - 0.991519
     - 0.000349
   * - Conv1D_LSTM
     - 0.991491
     - 0.000350
   * - LSTM_Attention
     - 0.979971
     - 0.000824

Key Observations
---------------

* All models achieved high R² scores (>0.97), indicating excellent predictive performance
* BiLSTM performed slightly better than the other models
* Training stopped early for all models (between 12-48 epochs)
* The notebook includes final comparison plots of all models

Saved Files
-----------

For each model:

* Model weights (``.pth``)
* Scaler object (``.joblib``)

Usage Notes
-----------

* Models were trained on CUDA-enabled hardware
* The notebook includes visualization of:
    * Training progress
    * Final results
* Early stopping was triggered based on validation loss