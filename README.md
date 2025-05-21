

# 🌬️ Wind Turbine Generator Predictive Maintenance Model

> ⚠️ **Note:** This model is based on a MATLAB Simulink example provided by MathWorks. It is used **strictly for educational purposes**.

## 📁 Project Files

* `Wind_Turbine.prj` — Main project file to initialize the workspace and set up paths.
* `Wind_Turbine.slx` — Simulink model of the wind turbine.
* `Wind_Turbine_Requirements.docx` — System-level documentation and requirements.
* `signal_data1.csv` — Exported signal data from simulations.

---

## 🔧 How to Use

1. Open `Wind_Turbine.prj` in MATLAB to initialize paths and environment.
2. Run the `Wind_Turbine_Demo_Script.html` to explore different parts of the simulation.
3. Modify environmental inputs (e.g., temperature, humidity, load cycles) to simulate wear and tear.
4. Observe failure conditions via scopes and sensor blocks within the Simulink model.

---

## ⚙️ Valid Configuration Notes

Some configurations are not valid and may cause runtime errors. Please ensure you use the following valid combinations:

| Turbine Input | Main Controller | Blade Loads             | Pitch Controller | Command Type        |
| ------------- | --------------- | ----------------------- | ---------------- | ------------------- |
| Inner Loop    | All             | Wind Input (not Torque) | On AoA           | Direct Input Torque |

> 🔄 The model `Wind_Turbine_Flexible_Blades.slx` is proof-of-concept only and is not integrated with `Wind_Turbine.slx`.

---

## 📉 Simulation Outputs & Failure Analysis

This simulation logs several operational parameters:

* Rotor speed
* Electromagnetic torque
* Vibration (RMS, kurtosis, crest factor)
* Degradation accumulation
* Environmental conditions (Temperature, Humidity)

Below are some of the failure behaviors encountered during development:

### 🌀 Rotor Speed Stability

![Rotor Speed Scope](./images/469f8cdb-5c38-413b-b279-61323549d15a.png)

> Rotor speed remains mostly stable, but occasional minor dips were observed due to actuator torque delay and incorrect cycle factor adjustment.

---

### ⚡ Electromagnetic Torque Fluctuations

![Electromagnetic Torque Scope](./images/ad90c1fd-568a-473d-98eb-49b912c8c819.png)

> Unexpected null torque in extended periods due to incorrect torque actuator dynamics. This led to energy inefficiency and poor vibration responses.

---

### 🛠️ Degradation Growth Over Time

![Degradation Scope](./images/754ff2ce-16e3-4b0c-a4de-e658699e241b.png)

> Gradual accumulation of degradation was observed, driven by temperature and humidity stressors. Failure to reset or cycle this factor caused rapid wear in long runs.

---

## 📦 Features

* Environmental degradation modeling via `tempData` and `humidityData`
* Sensor-based condition monitoring (vibration, torque, speed)
* Predictive maintenance metric output to CSV
* Custom MATLAB Function Block for degradation modeling
* Real-time signal plotting and export-ready analysis

---

## 📈 Potential Improvements

* Add fault injection models for gearbox and blade damage
* Integrate ML models for remaining useful life (RUL) estimation
* Improve cycle factor calculation based on actual turbine usage data
* Include real weather input from `.mat` or `.csv` datasets

---

## 📜 License & Credits

* Model © 2009–2020 The MathWorks, Inc.
* Extended and modified by \ Hachim & Abdellah
* For academic use only.


