# Uber Ride Analysis & Supply-Demand Optimization

An end-to-end data analytics project exploring Uber ride requests to uncover operational bottlenecks, cancellation hotspots, dynamic demand spikes, and the impact of external factors (such as weather and time of day) on ride completion rates.

---

## 📌 Project Overview
Ride-sharing platforms operate in a dynamic supply-demand ecosystem where driver availability and passenger requests fluctuate constantly. This project analyzes transactional ride request data to identify why trips fail—distinguishing between structural supply shortages (`No Cars Available`) and behavioral drop-offs (`Driver` vs. `Passenger` cancellations). 

The goal is to provide operational recommendations to improve trip completion rates, optimize driver allocation, and refine regional dispatch strategies.

---

## 🛠️ Data Pipeline & Technical Methods

### 1. Data Cleaning & Integrity Handling
* **Column Standardization:** Converted all raw column names to lowercase and replaced spaces with underscores for operational consistency.
* **Missing Value Imputation & Filtering:** 
  * Dropped records missing essential target metrics (`trip_status`, `trip_cost`)[cite: 1].
  * Replaced missing `payment_method` values with the statistical mode[cite: 1].
  * Imputed unassigned driver IDs with `-1` to explicitly track unfulfilled requests[cite: 1].
  * Screened out incomplete "Trip Completed" logs with missing start or drop-off timestamps[cite: 1].
* **Datetime Parsing:** Converted `request_timestamp`, `start_timestamp`, and `drop_timestamp` into pandas datetime objects for time-series extraction[cite: 1].

### 2. Outlier Detection & Treatment
Applied the **Interquartile Range (IQR)** method to mitigate extreme value skewing without losing record volume[cite: 1]:
* **Trip Cost & Extra Tip Capping:** Bounds were set at $Q1 - 1.5 \times IQR$ and $Q3 + 1.5 \times IQR$[cite: 1]. Extreme values beyond these thresholds were capped at the upper and lower bounds[cite: 1].

### 3. Feature Engineering
Derived granular temporal and categorical features to unlock deeper analytical dimensions[cite: 1]:
* **Duration Metrics:** Calculated `trip_duration_minutes` from trip start and drop-off timestamps[cite: 1].
* **Operational Delays:** Engineered `ride_delay` (in hours) measuring the lag between a user's `request_timestamp` and actual `start_timestamp`[cite: 1].
* **Temporal Deconstruction:** Extracted `request_date`, `request_day`, `request_time`, and `request_hour`[cite: 1].
* **Custom Cancellation Classification:** Programmatically categorized non-completed trips into `'No Cabs'`, `'Passenger'`, and `'Driver'` cancellations based on driver assignment status and trip outcome[cite: 1].

---

## 📊 Types of Analysis & Key Findings

### 1. Supply vs. Demand Bifurcation Analysis
* **Method:** Analyzed overall trip outcome proportions using pie charts and frequency bar plots[cite: 1].
* **Findings:** Isolated the overall fulfillment rate versus non-completion drivers[cite: 1]. The primary driver of incomplete trips is supply deficit (`No Cars Available`) followed closely by cancellations[cite: 1].

### 2. Temporal & Peak Window Analysis
* **Method:** Evaluated demand distributions across hours of the day (`request_hour`) and days of the week (`request_day`) using Seaborn count plots and stacked histograms[cite: 1].
* **Findings:** Identified distinct peak request hours where ride non-completion spikes due to driver shortages[cite: 1]. Driver cancellations also follow strong hourly patterns, concentrating during late-night and peak rush-hour slots[cite: 1].

### 3. Cancellation Hotspot & Route Mapping
* **Method:** Categorized incomplete trips by top `pickup_point` and `drop_point` locations[cite: 1].
* **Findings:** Highlighted specific high-volume pickup and drop-off points responsible for a disproportionate number of driver cancellations and unfulfilled requests[cite: 1].

### 4. Financial & Payment Preference Profiling
* **Method:** Grouped metrics by `payment_method` to evaluate distribution stats (`mean`, `median`, `count`) against `trip_cost` and overall `trip_status`[cite: 1].
* **Findings:** Determined average spending behavior per payment type, confirming which payment channels correlate with higher average trip values[cite: 1].

### 5. Environmental & Weather Impact Analysis
* **Method:** Grouped incomplete rides across `weather` conditions and custom `cancellation_reason` categories using grouped bar plots[cite: 1]. Evaluated average `trip_cost` across weather patterns[cite: 1].
* **Findings:** Adverse weather directly increases driver cancellation counts and correlates with shifts in mean trip costs, demonstrating the need for weather-contingent supply incentives[cite: 1].

### 6. Service Delay Impact Assessment
* **Method:** Built box plots and calculated statistical aggregations (`mean`, `median`) to measure how `ride_delay` influences ultimate trip success or failure[cite: 1].
* **Findings:** Long wait times post-request strongly correlate with trip abandonments and cancellations[cite: 1].

---

## 📂 Repository Structure
```text
├── Uber Ride Analysis.ipynb   # Complete execution notebook with data cleaning, EDA, and plots
├── uber_ride_analysis.py      # Python script version of the full analytical pipeline
├── README.md                  # Detailed project documentation
└── outputs/
    └── charts/                # Exported visual artifacts (Box plots, Histograms, Count plots)
