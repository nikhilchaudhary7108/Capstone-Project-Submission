# Capstone-Project-Submission

📄 Report: Dynamic Pricing for Urban Parking Lots
Project Title: Dynamic Pricing for Urban Parking Spaces
Submitted for: Summer Analytics 2025
Team/Student Name: Nikhil Chaudhary
Platform: Python, Pandas, Numpy, Pathway
Libraries Used: pandas, numpy, pathway, bokeh

🧠 Objective
This project aims to develop a real-time dynamic pricing engine for 14 urban parking spaces using simulated streaming data. The goal is to maximize efficient usage of parking lots by adjusting prices based on demand, traffic, queue length, and competition from nearby lots.

✅ Step-by-Step Approach
🔹 Step 1: Dataset Overview
The dataset contains 18,368 records spanning 14 parking lots across 73 days.

Each day has 18 time points (30-minute intervals from 8:00 AM to 4:30 PM).

Fields include occupancy, queue length, traffic condition, special events, and vehicle type.

🔹 Step 2: Model 1 – Baseline Linear Model
Formula:

Copy
Edit
Price[t+1] = Price[t] + α * (Occupancy / Capacity)
α = 5.0 (sensitivity parameter)

Starts from base price $10

Price clamped between $5 and $20

Increases linearly as occupancy increases

🔹 Step 3: Model 2 – Demand-Based Model
We introduced a custom demand function based on:

Occupancy ratio

Queue length

Traffic condition (mapped to numerical scale)

Special day indicator

Vehicle type weight

Demand Function:

ini
Copy
Edit
Demand = 5*(Occupancy/Capacity) + 2*Queue - 3*Traffic + 4*Special + 1*VehicleType
Then:

ini
Copy
Edit
Price = BasePrice × (1 + 0.8 × NormalizedDemand)
Demand was normalized using Min-Max scaling.

Prices remain smooth and bounded between $5 and $20.

🔹 Step 4: Model 3 – Competitive Pricing Model
This model integrates:

Location intelligence using Haversine distance

Nearby lot prices (within 1 km radius)

Demand-based price adjusted based on nearby lot pricing

Logic:

If lot is over 95% full and nearby lot is cheaper → reduce price or reroute

If nearby lots are expensive → slightly increase price

Else → retain Model 2’s demand-based price

Bonus: A reroute flag is added when the current lot is overburdened.

🧪 Real-Time Implementation: Pathway
We implemented the streaming logic using Pathway, simulating real-time price decisions with timestamp-aligned inputs. Our pricing logic runs per-record in real time and is integrated into a pw.io.csv.read → compute → pw.io.jsonlines.write pipeline.

📊 Visualizations using Bokeh
We used Bokeh to create the following real-time visualizations:

Line Plot: Price trends across time for each parking lot

Comparison Plot: Current lot vs nearby competitor prices

Reroute Flag Timeline: Highlights when rerouting is triggered

Include screenshots or embed interactive Bokeh plots in the Colab notebook or report.

📌 Assumptions Made
Demand scaling is based on expected ranges: max 100.

Occupancy > 95% is considered “full” for rerouting.

Competitor lot prices are averaged when determining influence.

Vehicle types have demand weights: cycle < bike < car < truck.

We assume each lot's location remains static.

🔁 How Price Responds to Features
Feature	Effect on Price
Occupancy ↑	Price ↑
Queue Length ↑	Price ↑
Traffic ↑	Price ↓
Special Day = 1	Price ↑
Vehicle Type (truck > cycle)	Price ↑
Nearby lots cheaper	Price ↓ or reroute
Nearby lots expensive	Price ↑

Prices evolve smoothly with demand and adjust dynamically with competition.

