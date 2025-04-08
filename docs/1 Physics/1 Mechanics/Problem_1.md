# Problem 1

**Investigating the Range as a Function of the Angle of Projection**

### **Motivation**
Projectile motion, while seemingly simple, provides a rich framework for exploring fundamental principles of physics. The problem at hand is straightforward: analyze how the range of a projectile depends on its angle of projection. Despite its simplicity, the topic involves complex relationships governed by both linear and quadratic equations, making it an insightful subject of study.

A key factor that makes this investigation compelling is the number of free parameters involved in projectile motion equations, such as initial velocity, gravitational acceleration, and launch height. These parameters contribute to a diverse set of solutions that model real-world scenarios, from the trajectory of a soccer ball to the flight path of a missile.

---

### **Task**

#### **1. Theoretical Foundation**
To analyze the range as a function of the angle of projection, we start by deriving the governing equations from fundamental principles of kinematics and dynamics. The motion of a projectile in the absence of air resistance follows Newton’s second law:

- **Equations of Motion:**
  - Horizontal displacement: $$ x = v_0 \cos(\theta) t $$
  - Vertical displacement: $$ y = v_0 \sin(\theta) t - \frac{1}{2}gt^2 $$

Solving these equations yields the total time of flight \( T \):
  $$ T = \frac{2v_0 \sin(\theta)}{g} $$

Substituting into the horizontal displacement equation, we obtain the range \( R \):
  $$ R = \frac{v_0^2 \sin(2\theta)}{g} $$

This equation shows that the range depends on the square of the initial velocity and the sine of twice the projection angle.

#### **2. Analysis of the Range**
- The range \( R \) is maximized when \( \sin(2\theta) \) reaches its maximum value (1), which occurs at \( 2\theta = 90^\circ \), or \( \theta = 45^\circ \).
- Increasing the initial velocity \( v_0 \) increases the range quadratically.
- A higher gravitational acceleration \( g \) reduces the range.
- For non-zero launch height, the equation for \( R \) becomes more complex, requiring further analysis.

#### **3. Practical Applications**
- **Sports:** Understanding how the angle influences projectile range helps optimize techniques in soccer, basketball, and javelin throwing.
- **Engineering:** In artillery and missile technology, precise control over launch angles is essential for maximizing impact range.
- **Space Exploration:** The principles of projectile motion extend to orbital mechanics and spacecraft trajectory planning.

#### **4. Implementation**
To further analyze the range as a function of the projection angle, a computational tool can be developed:
- **Algorithm:** Implement numerical simulations in Python or MATLAB to visualize projectile motion under varying conditions.
- **Graphical Representation:** Generate plots of range versus launch angle to observe the dependency.
- **Advanced Considerations:** Extend the model to include air resistance, variable wind conditions, and non-flat terrain.

---

### **Conclusion**
The study of projectile motion offers valuable insights into both theoretical physics and practical applications. By understanding the mathematical framework and leveraging computational tools, we can develop a deeper appreciation of how varying initial conditions affect a projectile’s trajectory. Future work may involve refining the model with real-world complexities such as air resistance and varying gravitational fields.

![alt text](image.png)

### Interactive Simulation

<details>
<summary>Click to interact with parameters</summary>

<div style="padding: 15px; border: 1px solid #e1e4e8; border-radius: 6px; margin-top: 10px;">
    <div id="plotly-chart"></div>
    <div style="margin-top: 20px;">
        <label style="display: block; margin: 10px 0;">
            Initial Velocity (m/s): 
            <input type="range" id="v0" min="5" max="50" value="20" style="vertical-align: middle;">
            <span id="v0Value">20</span>
        </label>
        <label style="display: block; margin: 10px 0;">
            Gravity (m/s²): 
            <input type="range" id="gravity" min="3" max="25" value="9.81" step="0.1">
            <span id="gravityValue">9.81</span>
        </label>
        <label style="display: block; margin: 10px 0;">
            Launch Height (m): 
            <input type="range" id="height" min="0" max="30" value="0">
            <span id="heightValue">0</span>
        </label>
    </div>
</div>

<script src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>
<script>
(function() {
    // Calculation and plotting logic
    function calculateRange(theta, v0, g, h) {
        const thetaRad = theta * Math.PI / 180;
        const sinTheta = Math.sin(thetaRad);
        const cosTheta = Math.cos(thetaRad);
        const discriminant = (v0*sinTheta)**2 + 2*g*h;
        if(discriminant < 0) return 0;
        const t = (v0*sinTheta + Math.sqrt(discriminant))/g;
        return v0*cosTheta * t;
    }

    function updatePlot() {
        const v0 = parseFloat(document.getElementById('v0').value);
        const g = parseFloat(document.getElementById('gravity').value);
        const h = parseFloat(document.getElementById('height').value);
        
        const angles = Array.from({length: 90}, (_, i) => i);
        const ranges = angles.map(angle => calculateRange(angle, v0, g, h));

        Plotly.newPlot('plotly-chart', [{
            x: angles,
            y: ranges,
            type: 'scatter',
            mode: 'lines+markers'
        }], {
            title: `Range vs. Launch Angle (v₀=${v0}m/s, g=${g}m/s², h=${h}m)`,
            xaxis: {title: 'Launch Angle (degrees)'},
            yaxis: {title: 'Range (m)'}
        });
    }

    // Event listeners
    document.querySelectorAll('input[type="range"]').forEach(input => {
        input.addEventListener('input', function() {
            document.getElementById(this.id + 'Value').textContent = this.value;
            updatePlot();
        });
    });

    // Initial plot
    updatePlot();
})();
</script>
</details>