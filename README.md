#  Dynamic Parking Pricing System

A real-time data streaming project that dynamically adjusts parking prices based on demand. This project uses **Pathway** to build two pricing models that analyze live occupancy, traffic, and vehicle flow data to generate fair and adaptive pricing strategies.

##  Tech Stack Used

- **Python**: Data manipulation and pipeline logic
- **Pathway**: Real-time data processing and windowing
- **Pandas**: Preprocessing for static data
- **Panel + Bokeh**: Interactive visualizations and dashboards
- **Google Colab**: Development and testing environment
- **Git & GitHub**: Version control and submission

## Architecture Diagram

![Architecture Diagram](Architecture%20Diagram.png)

##  Detailed Explanation of Project Architecture and Workflow

The project follows a modular, event-driven architecture suitable for real-time price prediction in dynamic parking environments. The key stages of the workflow are described below:


###  1. Data Ingestion
- **Source:** A cleaned CSV file containing historical parking data.
- **Streaming:** The data is simulated as a real-time stream using Pathway’s `replay_csv` method.
- **Input Rate:** Adjustable to control how many rows are streamed per second (e.g., 100 rows/sec).


###  2. Data Preprocessing
- **Timestamp Parsing:** Converts string timestamps to datetime format.
- **Feature Engineering:** Computes `occupancy_rate` and encodes categorical features like `TrafficConditionNearby` and `VehicleType`.
- **Columns Selected:** Only relevant features such as `Occupancy`, `Capacity`, `QueueLength`, and encoded values are retained.


###  3. Session Windowing
- **Session Windows:** Data is segmented using Pathway’s `session` window of 0.3 hours (~18 minutes) for each parking lot.
- **Aggregation:** For each session, max values of occupancy, queue length, etc., are computed.


###  4. Pricing Models

#### Model 1: Baseline Linear Price Model

**Formula:**  
`Price_t = 10 + α × (Occupancy / Capacity)`

- Simple linear relationship with occupancy rate  
- Constant α = 0.4  
- Stateless: Price is recomputed per session from base price  

---

#### Model 2: Demand-Based Pricing

**Formula:**  
`Price_t = BasePrice × (1 + λ × NormalizedDemand)`

**Demand Function:**  
`Demand = α × (Occupancy / Capacity) + β × QueueLength - γ × Traffic + δ × IsSpecialDay + ε × VehicleType`

- Demand is normalized to [0, 1]  
- Price is capped between ₹5 and ₹20  
- Constants: α = 0.4, β = 0.3, γ = 0.2, δ = 0.2, ε = 0.5, λ = 1.0



###  5. Visualization
- **Tool:** Bokeh + Panel for interactive plotting in real-time.
- **Setup:** Separate plots for selected parking lots.
- **Features Displayed:** Price over time for both models.


###  Summary
- The project simulates a real-world dynamic pricing scenario using live dataflow.
- Pathway enables low-latency updates and efficient windowed computation.
- The code is modular and easily extendable to other pricing strategies or datasets.

💡 To view the interactive visualizations, run the notebooks in this repository on Google Colab.
