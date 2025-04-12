# ✈️ Airline Overbooking Optimization via Dynamic Programming

## 📄 Overview

This project applies dynamic programming to optimize airline overbooking policies for coach and first-class tickets over a 365-day selling horizon. It considers pricing decisions, overbooking levels, no-sale options, and seasonal demand effects. We evaluate and simulate multiple policies to determine the most profitable and robust approach.

---

## 🧠 Problem Statements and Approaches

### 🧩 Problem 1: Fixed Overbooking Policy (+5 Seats)

- **Objective**: Maximize expected discounted profit when coach is oversold by 5 seats (total 105).
- **Method**: Bellman recursion with 4 pricing options; show-up modeled as binomial distribution (95% coach, 97% first-class).
- **Result**:  
  `Expected Profit: $41,886.16`

---

### 🧩 Problem 2: Optimal Fixed Overbooking Level

- **Objective**: Evaluate overbooking from +6 to +15 seats to find maximum profit.
- **Result**:  
  `Optimal Overbooking: +9 coach seats`  
  `Expected Profit: $42,134.62`

---

### 🧩 Problem 3: No-Sale Option for Coach

- **Objective**: Add option to skip selling coach tickets on a day.
- **Decision Set**: Coach: $300, $350, No Sale; First-class: $425, $500.
- **Max Coach Cap**: 120
- **Result**:  
  `Expected Profit: $42,139.89`

---

### 🧩 Problem 4: Seasonality in Demand

- **Objective**: Introduce seasonality by increasing sale probabilities as departure date approaches.
- **Method**: Probability multiplier = `0.75 + t / 730` where t = day index.
- **Result**:  
  `Optimal Overbooking: +16 to +20 seats`  
  `Expected Profit: $41,841.11`

---

### 🧩 Problem 5: Forward Simulation (10,000 Runs)

- **Objective**: Compare Policy 1 (Fixed) vs Policy 2 (Seasonal + No Sale) under stochastic demand.
- **Findings**:
  - Policy 1: higher profit, lower volatility, lower overbooking cost.
  - Policy 2: performs better under stressed conditions (higher coach attendance rate).
- **Recommendation**:  
  Use **Policy 1** under normal demand  
  Use **Policy 2** under high-attendance stress
  
---

## 📊 Key Metrics Summary

### 🔑 Profit Comparison by Policy

| Policy              | Overbooking Level | Expected Profit |
|---------------------|-------------------|------------------|
| Fixed (+5)          | 105 seats         | $41,886.16       |
| Optimal Fixed       | 109 seats         | $42,134.62       |
| No-Sale Option      | 120 seats         | $42,139.89       |
| Seasonality         | 116–120 seats     | $41,841.11       |

---

### 📈 Stress Test 1: Coach Attendance Rate Increased (95% → 98%)

| Metric                  | Policy 1 (Base) | Policy 1 (Stress) | Policy 2 (Base) | Policy 2 (Stress) |
|-------------------------|----------------|-------------------|----------------|-------------------|
| Avg. Profit ($)         | 41,989.29      | 40,750.07         | 41,687.46      | 40,621.80         |
| Std Dev Profit          | 998.39         | 867.91            | 1,001.53       | 1,021.11          |
| Avg. Overbooking Cost   | 976.63         | 2,207.95          | 990.92         | 2,177.34          |
| Overbooking Frequency % | 81.33%         | 99.45%            | 81.93%         | 99.5%             |
| Avg. Passengers Rejected| 2.21           | 5.08              | 2.23           | 5.00              |

---

### 📈 Stress Test 2: First-Class Attendance Rate Increased (97% → 100%)

| Metric                  | Policy 1 (Base) | Policy 1 (Stress) | Policy 2 (Base) | Policy 2 (Stress) |
|-------------------------|----------------|-------------------|----------------|-------------------|
| Avg. Profit ($)         | 41,989.29      | 41,874.21         | 41,687.46      | 41,718.04         |
| Std Dev Profit          | 998.39         | 1,015.13          | 1,001.53       | 1,120.20          |
| Avg. Overbooking Cost   | 976.63         | 1,082.73          | 990.92         | 1,086.20          |
| Overbooking Frequency % | 81.33%         | 81.22%            | 81.93%         | 82.51%            |
| Avg. Passengers Rejected| 2.21           | 2.49              | 2.23           | 2.50              |

---

## 🧮 Model Implementation

### 🔄 Bellman Equation (V-function)
```python
@lru_cache(maxsize=None)
def V(d, s_c, s_f):
    if d == 0:
        return -compute_terminal_cost(s_c, s_f)
    ...
    return best_val

---


