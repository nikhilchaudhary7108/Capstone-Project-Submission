# 🚗 Dynamic Pricing for Urban Parking Lots (Summer Analytics 2025)

> An intelligent, real-time pricing engine for 14 urban parking lots using demand modeling, spatial competition, and real-time data streaming.

---

## 🧠 Overview

This project simulates dynamic pricing of limited-capacity parking spaces in an urban environment. The goal is to optimize utilization by adjusting prices in real-time based on factors such as demand, queue length, traffic, special events, vehicle type, and competitive prices from nearby parking lots.

**Key Features:**
- Three staged pricing models (Linear, Demand-based, Competitive)
- Real-time data simulation and pricing via [Pathway](https://pathway.com/)
- Real-time visualization using Bokeh
- Rerouting logic for overburdened lots

---

## 🧰 Tech Stack

| Component              | Technology                     |
|------------------------|---------------------------------|
| Data Processing        | Python, Pandas, NumPy           |
| Real-Time Engine       | [Pathway](https://pathway.com/) |
| Visualization          | Bokeh                           |
| Distance Calculation   | Haversine Formula               |
| IDE / Platform         | Google Colab                    |
| Data Format            | CSV (Input), JSONL (Output)     |

---

## 🏗️ Architecture Diagram 


flowchart TD
    A[Dataset.csv] --> B[Pathway Streaming Reader]
    B --> C{Pricing Logic}
    C -->|Model 1| D1[Baseline Linear Model]
    C -->|Model 2| D2[Demand-Based Model]
    C -->|Model 3| D3[Competitive + Rerouting]
    D1 --> E[Streamed Price Output]
    D2 --> E
    D3 --> E
    E --> F[Bokeh Visualization]
    F --> G[Colab / Report Output]


Project Workflow
Data Ingestion

Read CSV with 14 parking lots and 18 time intervals/day.

Parsed into a Pathway streaming pipeline with timestamp ordering.

Model 1 – Baseline Linear Model

Price increases linearly with occupancy ratio.

Acts as a control/reference model.

Model 2 – Demand-Based Model

Constructs a weighted demand function using:

Occupancy / Capacity

Queue Length

Traffic Level

Special Event flag

Vehicle type

Model 3 – Competitive Model

Adds spatial awareness using Haversine distance.

Adjusts price based on proximity to cheaper/expensive nearby lots.

Triggers reroute suggestion if overcapacity and better nearby options exist.

Streaming Output

Final prices are streamed and logged to pricing_output.jsonl.

Visualization

Bokeh is used to show:

Price trends

Lot comparison

Reroute flags on timeline

Reporting

Complete documentation and visualizations included in final Google Colab and PDF report.
