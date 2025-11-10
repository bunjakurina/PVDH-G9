# Data Preparation and Visualization — Electric Vehicle Specifications  

### Introductory Information  
**University of Prishtina — Faculty of Computer and Software Engineering**  
**Master’s Program in Computer and Software Engineering**  
**Professor:** Dr.Sc. Mërgim Hoti  
**Group:** 9  

---

### Team Members  
- Rina Bunjaku
- Uranik Hodaj 

---

### Project Overview  
Performance and efficiency analysis of Electric Vehicles based on technical characteristics.
This project focuses on the **analysis and visualization of electric vehicle specifications** to identify structured specifications of electric vehicles (EVs), including brand, model, battery, charging, range, performance, and size metrics

---

### Dataset  

**Dataset:** Electric Vehicles Specifications Dataset  
**Source:**  — <a href="https://www.kaggle.com/datasets/urvishahir/electric-vehicle-specifications-dataset-2025?resource=download"> Kaggle</a>
---

### Project Setup  



```bash
# Create project directory
mkdir EV-Visualization
cd EV-Visualization

# Clone the repository
git clone https://github.com/bunjakurina/PVDH-G9.git
cd EV-Visualization

# Create and Activate a Virtual Environment
python -m venv venv
venv\Scripts\activate

# Install Dependencies
pip install -r requirements.txt

#Launch Jupyter Notebook
Option A — From VS Code

Install the Jupyter extension.

Open the .ipynb file.

Make sure the interpreter is set to your virtual environment (venv).

Option B — From Terminal
jupyter notebook
```

# Results

---

## Defining Data Types

- **Total rows:** 478  
- **Total columns:** 22  

### Categorical variables  
`brand`, `model`, `battery_type`, `fast_charge_port`, `cargo_volume_l`, `drivetrain`, `segment`, `car_body_type`, `source_url`

### Numerical variables  
`top_speed_kmh`, `battery_capacity_kWh`, `number_of_cells`, `torque_nm`, `efficiency_wh_per_km`, `range_km`, `acceleration_0_100_s`, `fast_charging_power_kw_dc`, `towing_capacity_kg`, `seats`, `length_mm`, `width_mm`, `height_mm`

---

## Data Cleaning

- **Columns removed (not relevant):**  
  `number_of_cells`, `length_mm`, `width_mm`, `height_mm`, `source_url`  
- **Columns with missing values:**  
  `model`, `torque_nm`, `fast_charging_power_kw_dc`, `fast_charge_port`, `towing_capacity_kg`, `cargo_volume_l`
- **Rows containing missing data:** 36  
- **Indices with NaN:** `[4, 5, 61, 100, 101, 102, ... 477]`
- After filling with mean/median/mode → **Total missing values:** `37 → 0`
- **Duplicated entries:** `0`
- **Invalid values (checked for extremes):** none outside realistic range  
  (e.g., no top speeds >400 km/h or battery capacities >200 kWh)

---

## Encoded Data (Label Encoding)

Categorical columns were encoded numerically.

| brand | model | top_speed_kmh | battery_capacity_kWh | torque_nm | efficiency_wh_per_km | range_km | drivetrain | car_body_type |
|-------|--------|----------------|----------------------|------------|----------------------|-----------|-------------|----------------|
| 0 | 46 | 155 | 37.8 | 235.0 | 156 | 225 | 1 | 2 |
| 1 | 396 | 150 | 60.0 | 310.0 | 156 | 315 | 1 | 4 |

**NaN values:** Reduced from `34 → 0` after encoding.  
**Duplicates:** `0` found.

---

## Aggregation & Summary Statistics

### Average Range by Car Body Type

| Car Body Type | Average Range (km) |
|----------------|-------------------:|
| Cabriolet | 316 |
| Coupe | 442.5 |
| Hatchback | 302.8 |
| Liftback Sedan | 488.2 |
| SUV | 396.5 |

### Average Battery Capacity by Drivetrain

| Drivetrain | Avg. Battery (kWh) |
|-------------|-------------------:|
| AWD | 88.58 |
| FWD | 55.50 |
| RWD | 74.93 |

### Average Efficiency (Wh/km) by Car Body Type

| Car Body Type | Avg. Efficiency (Wh/km) |
|----------------|-------------------------:|
| Cabriolet | 149.8 |
| Coupe | 187.0 |
| Hatchback | 133.5 |
| SUV | 161.5 |
| Sedan | 161.2 |
| Small Passenger Van | 212.4 |
| Station/Estate | 165.3 |

### Average Top Speed by Brand (sample)

| Brand | Avg. Top Speed (km/h) |
|--------|----------------------:|
| Abarth | 150 |
| Aiways | 160 |
| Audi | 230 |
| BMW | 250 |
| BYD | 180 |

### Models per Brand & Car Body Type (sample)

| Brand | Car Body Type | Num. Models |
|--------|----------------|-------------:|
| Audi | SUV | 6 |
| BMW | Sedan | 5 |
| Tesla | Liftback Sedan | 4 |
| BYD | SUV | 4 |
| Renault | Hatchback | 3 |

---

## Sampling

- **Total rows:** 478  
- **Simple sample (10%)** → 48 rows  
- **Fixed sample (100 rows)** created  
- **Stratified sample by Car Body Type (20%)** maintained proportional representation

Example of stratified distribution (approximate):  
SUV ≈ 52%, Sedan ≈ 14%, Hatchback ≈ 12%, etc.

---

## Feature Selection & Correlation

Highly correlated features were identified using a 0.7 threshold.  
Subset used for analysis:  
`battery_capacity_kWh`, `range_km`, `efficiency_wh_per_km`, `fast_charging_power_kw_dc`, `top_speed_kmh`

---

## Derived Features

New engineered attributes:

| Feature | Description |
|----------|--------------|
| `range_per_kWh` | Efficiency in km per kWh |
| `speed_efficiency_ratio` | Ratio of top speed to efficiency |
| `performance_index` | Top speed divided by acceleration time |
| `charge_rate_kw_per_kWh` | Fast charge rate relative to capacity |
| `eco_score` | Composite of range, efficiency, and acceleration |
| `range_category` | Categorized as Shkurt / Mesatar / Gjatë / Shumë Gjatë |

**Example:**

| brand | model | range_per_kWh | performance_index | eco_score | range_category |
|--------|--------|---------------|-------------------|-----------|----------------|
| Abarth | 500e Convertible | 5.95 | 22.14 | 3.30 | Shkurt |
| Abarth | 600e Turismo | 5.51 | 32.26 | 3.16 | Shkurt |
| Aiways | U5 | 5.25 | 20.00 | 2.94 | Mesatar |

---

## Discretization & Binarization

Battery capacity and range were discretized into 5 bins and binarized.

| Battery (kWh) | Battery Bin | Range (km) | Range Bin | Battery Bin. | Range Bin. |
|----------------|-------------|-------------|------------|----------------|-------------:|
| 37.8 | 1 | 225 | 1 | 1 | 1 |
| 60.0 | 2 | 315 | 2 | 1 | 1 |
| 88.5 | 3 | 440 | 3 | 1 | 1 |
| 110.0 | 4 | 520 | 4 | 1 | 1 |
| 150.0 | 5 | 720 | 5 | 1 | 1 |

---

## PCA (Principal Component Analysis)

- **Explained variance per component:**
  - PC1: 47.46%
  - PC2: 23.92%
  - PC3: 8.46%
  - PC4: 5.80%
- **Total variance retained:** 85.64%  

PCA reduced the dataset to **4 dimensions**, preserving most of the information.

---

## Standardization & Normalization

All numerical features were standardized (mean = 0, std = 1)  
and normalized (scaled between 0–1) for uniformity.

**Standardized Sample (first 5 rows):**
Values centered around zero, with similar variance.  

**Normalized Sample (first 5 rows):**
All features scaled between 0–1.

---

## Vehicle Distribution by Car Body Type

| Car Body Type | Proportion |
|----------------|------------:|
| SUV | 51.6% |
| Sedan | 13.7% |
| Hatchback | 11.6% |
| Small Passenger Van | 9.5% |
| Liftback Sedan | 7.4% |
| Station/Estate | 5.3% |
| Cabriolet | 1.1% |

---

## Top Performing Vehicles

| Brand | Model | Range (km) | Estimated Range | % Difference |
|--------|--------|------------|-----------------|--------------:|
| Zeekr | 001 Performance AWD | 480 | 591.19 | +23.17% |
| Audi | SQ6 e-tron Sportback | 495 | 578.66 | +16.90% |
| Ford | e-Tourneo Custom L2 160 kW | 235 | 172.97 | -26.39% |
| BYD | SEALION 7 82.5 kWh RWD Comfort | 415 | 482.46 | +16.25% |
| CUPRA | Tavascan Endurance | 445 | 542.25 | +21.85% |

---

## Summary

 Dataset cleaned and processed  
 Missing and duplicate values handled  
 Numerical and categorical features encoded  
 New analytical variables created (`eco_score`, `performance_index`, etc.)  
 Data standardized, normalized, and reduced via PCA  
 Aggregations reveal relationships between **drivetrain efficiency**, **battery capacity**, and **car body type**  
 Final dataset exported as `electric_vehicles_preproccesed.csv`



 
