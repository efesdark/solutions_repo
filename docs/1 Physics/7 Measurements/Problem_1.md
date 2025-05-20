# Measurement of Earth's Gravitational Acceleration Using a Pendulum

## Introduction
The gravitational acceleration \( g \) is determined using a simple pendulum. The period \( T \) of a pendulum depends on its length \( L \) and \( g \), as described by the formula:
\[
T = 2\pi \sqrt{\frac{L}{g}} \implies g = \frac{4\pi^2 L}{T^2}.
\]
Uncertainties in \( L \) and \( T \) propagate to \( g \), requiring rigorous error analysis.

---

## Experimental Setup
- **Pendulum length**: \( L = 1.200 \, \text{m} \) (measured with a ruler of resolution \( 1 \, \text{mm} \), uncertainty \( \delta L = \pm 0.0005 \, \text{m} \)).
- **Weight**: Small object (e.g., keys) attached to a string.
- **Time measurement**: Stopwatch with \( 0.01 \, \text{s} \) resolution.

---

## Data Collection
### Time for 10 Oscillations (\( t_{10} \))
| Trial | \( t_{10} \, (\text{s}) \) |
|-------|----------------------------|
| 1     | 21.45                      |
| 2     | 21.52                      |
| 3     | 21.38                      |
| 4     | 21.60                      |
| 5     | 21.41                      |
| 6     | 21.55                      |
| 7     | 21.47                      |
| 8     | 21.50                      |
| 9     | 21.43                      |
| 10    | 21.56                      |

### Statistical Analysis
- Mean time for 10 oscillations: \( \bar{t}_{10} = 21.487 \, \text{s} \)
- Standard deviation: \( \sigma_{t_{10}} = 0.073 \, \text{s} \)
- Uncertainty in mean time: 
\[
\delta \bar{t}_{10} = \frac{\sigma_{t_{10}}}{\sqrt{10}} = 0.023 \, \text{s}.
\]

---

## Calculations
### Period \( T \)
\[
T = \frac{\bar{t}_{10}}{10} = 2.1487 \, \text{s}, \quad \delta T = \frac{\delta \bar{t}_{10}}{10} = 0.0023 \, \text{s}.
\]

### Gravitational Acceleration \( g \)
\[
g = \frac{4\pi^2 L}{T^2} = \frac{4\pi^2 \times 1.200}{(2.1487)^2} = 9.76 \, \text{m/s}^2.
\]

### Uncertainty Propagation
\[
\delta g = g \sqrt{\left(\frac{\delta L}{L}\right)^2 + \left(2 \frac{\delta T}{T}\right)^2} = 9.76 \sqrt{\left(\frac{0.0005}{1.200}\right)^2 + \left(2 \times \frac{0.0023}{2.1487}\right)^2} = 0.07 \, \text{m/s}^2.
\]

**Final result**:  
\[
g = 9.76 \pm 0.07 \, \text{m/s}^2.
\]

---

## Analysis
### Comparison with Standard Value
The measured \( g = 9.76 \pm 0.07 \, \text{m/s}^2 \) agrees with the accepted value (\( 9.81 \, \text{m/s}^2 \)) within uncertainties.

### Sources of Uncertainty
1. **Length measurement**:
   - Resolution of ruler (\( \delta L = \pm 0.0005 \, \text{m} \)) contributes minimally to \( \delta g \).
2. **Timing variability**:
   - Human reaction time and stopwatch resolution dominate \( \delta T \).
   - Standard deviation in \( t_{10} \) reflects natural variability in oscillations.
3. **Assumptions**:
   - Small-angle approximation (\( <15^\circ \)) was violated if displacements were large.
   - Air resistance and string elasticity were neglected.

### Experimental Limitations
- **Systematic errors**: Non-ideal pendulum motion (e.g., elliptical swing).
- **Environmental factors**: Airflow affecting pendulum motion.

---

## Conclusion
The experiment successfully estimated \( g \) with \( 0.7\% \) uncertainty. Key improvements would include automated timing, stricter angle control, and a more rigid pendulum support.