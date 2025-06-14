Wind Turbine Degradation Simulator Documentation
===============================================

Overview
--------

The ``WindTurbineDegradationSimulator`` class simulates the degradation of a wind turbine generator system over time based on operational conditions including wind speed, temperature, humidity, and vibration. The model incorporates:

* Aerodynamic power calculations
* Mechanical torque and speed computations
* Multi-factor degradation modeling
* Maintenance event simulation
* Comprehensive analysis and visualization

Class Initialization
--------------------

.. py:class:: WindTurbineDegradationSimulator()
   :noindex:

   Initializes the wind turbine simulator with default parameters and state variables.

   **Parameters**:

   * ``params``: Dictionary containing turbine specifications and operational parameters
     * Blade/diameter specs (``blade_length``, ``rotor_diameter``)
     * Power characteristics (``rated_power``, ``max_power``)
     * Speed thresholds (``cut_in_speed``, ``rated_speed``, ``cut_out_speed``)
     * Mechanical specs (``tip_speed``, ``gearbox_ratio``)
     * Electrical specs (``voltage``, ``frequency``, ``poles``)
     * Maintenance parameters (``maintenance_threshold``, ``maintenance_effectiveness``)

   **State Variables**:

   * ``degradation_value``: Current degradation level (0-1)
   * ``time_since_last_maintenance``: Steps since last maintenance
   * ``maintenance_events_count``: Total maintenance events performed

Core Calculation Methods
-----------------------

.. py:method:: calculate_rotor_rpm(wind_speed)
   :noindex:

   Calculates rotor RPM based on wind speed and tip speed constraints.

   :param wind_speed: Current wind speed in m/s
   :return: Rotor speed in RPM (0 if below cut-in or above cut-out speed)

.. py:method:: calculate_cp(tsr, pitch_angle=0)
   :noindex:

   Computes the power coefficient (Cp) using an improved wind turbine model.

   :param tsr: Tip-speed ratio
   :param pitch_angle: Blade pitch angle in degrees (default 0)
   :return: Power coefficient (clipped between 0.0 and 0.48)

.. py:method:: calculate_system_torque(wind_speed)
   :noindex:

   Calculates the mechanical system torque and generator speed.

   :param wind_speed: Current wind speed in m/s
   :return: Tuple of (system_torque_knm, generator_speed_rpm)
            Returns (0, 0) if wind speed is outside operating range

Degradation Modeling
-------------------

.. py:method:: simple_cycle_factor(temp_data_input)
   :noindex:

   Calculates a fatigue factor based on temperature fluctuations.

   :param temp_data_input: Time series of temperature values
   :return: Cycle factor representing thermal fatigue contribution

.. py:method:: wind_turbine_generator_degradation(torque, speed, vibration, temp, humidity, cycle_factor, current_time_step)
   :noindex:

   Calculates degradation increase for a single time step and handles maintenance.

   :param torque: System torque in kNm
   :param speed: Generator speed in RPM
   :param vibration: Vibration level
   :param temp: Temperature in °C
   :param humidity: Relative humidity in %
   :param cycle_factor: Thermal fatigue factor
   :param current_time_step: Current simulation step

   **Degradation Factors**:

   * Mechanical wear (torque × speed)
   * Vibration stress
   * Thermal stress (Arrhenius model)
   * Humidity stress
   * Thermal cycle stress

   **Maintenance Logic**:

   * Triggers when degradation exceeds threshold and sufficient time has passed
   * Reduces degradation by effectiveness factor
   * Resets maintenance timer

   :return: Updated degradation value (0-1)

Data Processing
--------------

.. py:method:: process_time_series(csv_file_path=None, df=None)
   :noindex:

   Processes input time series data to calculate torque, speed, and degradation.

   :param csv_file_path: Path to CSV file (optional)
   :param df: DataFrame with input data (optional)

   **Input Data Handling**:

   * Accepts either wind 'speed' column or 'u'/'v' components
   * Handles missing vibration/humidity data with defaults
   * Generates synthetic temperature data if missing

   **Outputs**:

   DataFrame with original data plus:

   * ``system_torque_knm``
   * ``generator_speed_rpm``
   * ``degradation``
   * ``cycle_factor``
   * ``maintenance_performed``
   * ``time_since_last_maintenance``

Analysis Methods
---------------

.. py:method:: plot_degradation_analysis(df, save_path=None)
   :noindex:

   Generates comprehensive diagnostic plots.

   **Plots Include**:

   * Degradation evolution with maintenance events
   * Wind speed vs. system torque
   * Temperature profile
   * Torque over time
   * Humidity vs. degradation rate
   * Generator speed distribution

.. py:method:: print_degradation_summary(df)
   :noindex:

   Prints key statistics from simulation.

   **Output Includes**:

   * Final degradation level
   * Maintenance events count
   * Time since last maintenance
   * Cycle factor impact

Usage Example
------------

.. code-block:: python

   simulator = WindTurbineDegradationSimulator()

   # Process data from CSV
   results = simulator.process_time_series(csv_file_path="wind_data.csv")

   # Or process DataFrame directly
   results = simulator.process_time_series(df=my_dataframe)

   # Generate analysis
   simulator.print_degradation_summary(results)
   simulator.plot_degradation_analysis(results, save_path="analysis.png")