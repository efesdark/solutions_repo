# Measurements

## Problem 1: Measuring Earth's Gravitational Acceleration with a Pendulum

### Motivation

The acceleration due to gravity \( g \) is a fundamental constant that influences a wide range of physical phenomena. Accurate measurement of \( g \) is essential for understanding gravitational interactions, designing structures, and conducting experiments in physics. One classical method for determining \( g \) involves measuring the oscillation period of a simple pendulum.

This experiment emphasizes rigorous measurement procedures, uncertainty analysis, and their importance in experimental physics.

---

### Task

Measure the gravitational acceleration \( g \) using a pendulum, and analyze the uncertainties in your measurements in detail.

---

### Procedure

#### 1. Materials

- A string (1 to 1.5 meters in length)
- A small mass (e.g., a bag of coins, keychain, or similar)
- Stopwatch or smartphone timer
- Ruler or measuring tape

#### 2. Setup

- Attach the mass to the end of the string and secure the other end to a fixed support.
- Measure the length \( L \) of the pendulum from the point of suspension to the center of mass. 
- Record the resolution of the measuring instrument and calculate the length uncertainty as \( \Delta L = \frac{\text{resolution}}{2} \).

#### 3. Data Collection

- Displace the pendulum by less than 15° and release.
- Measure the total time \( t_{10} \) for 10 full oscillations.
- Repeat this process 10 times and record all measurements.
- Calculate the average time \( \overline{t}_{10} \) and the standard deviation \( \sigma \).
- Determine the uncertainty in the average time using:
  
  \[
  \Delta \overline{t}_{10} = \frac{\sigma}{\sqrt{n}}
  \]

---

### Calculations

#### 1. Period Calculation

\[
T = \frac{\overline{t}_{10}}{10}
\]

\[
\Delta T = \frac{\Delta \overline{t}_{10}}{10}
\]

#### 2. Gravitational Acceleration

\[
g = \frac{4\pi^2 L}{T^2}
\]

#### 3. Uncertainty Propagation

Use the propagation of uncertainty formula:

\[
\left( \frac{\Delta g}{g} \right)^2 = \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \cdot \frac{\Delta T}{T} \right)^2
\]

\[
\Delta g = g \cdot \sqrt{ \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \cdot \frac{\Delta T}{T} \right)^2 }
\]

---

### Analysis

#### 1. Comparison

- Compare your measured value of \( g \) with the standard gravitational acceleration \( g_0 = 9.80665 \, \text{m/s}^2 \).

#### 2. Discussion

- How does the resolution of the measuring instruments affect \( g \)?
- What is the impact of variation in timing measurements?
- Discuss the assumptions made (small angle approximation, air resistance, etc.) and potential sources of error.

---

### Deliverables

#### 1. Tabulated Data

| Trial | \( t_{10} \) [s] | \( T \) [s] | |
|-------|------------------|-------------|--|
| 1     | ...              | ...         |  |
| 2     | ...              | ...         |  |
| ...   | ...              | ...         |  |
| 10    | ...              | ...         |  |

- \( \overline{t}_{10} = ... \)
- \( \sigma = ... \)
- \( \Delta \overline{t}_{10} = ... \)
- \( T = ... \pm \Delta T \)
- \( L = ... \pm \Delta L \)
- \( g = ... \pm \Delta g \)

#### 2. Discussion Section

Provide a brief but insightful discussion of the uncertainty sources and their contributions to your final result.

---

**Keywords:** Pendulum, Gravitational Acceleration, Experimental Physics, Measurement Uncertainty, Period, Oscillations
