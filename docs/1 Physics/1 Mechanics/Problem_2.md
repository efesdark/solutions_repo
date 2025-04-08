# Problem 2

**Investigating the Dynamics of a Forced Damped Pendulum**

## Motivation
The forced damped pendulum exemplifies complex dynamics emerging from the interplay of damping, restoring, and driving forces. This system exhibits phenomena ranging from resonance to chaos, making it fundamental for understanding oscillatory systems in engineering and physics.

---

## Theoretical Foundation

### Governing Equation
The motion is described by:
$$
\frac{d^2\theta}{dt^2} + \frac{b}{m}\frac{d\theta}{dt} + \frac{g}{L}\sin\theta = \frac{F}{mL}\cos(\omega_d t)
$$

**Linearized form (small angles):**
$$
\frac{d^2\theta}{dt^2} + 2\beta\omega_0\frac{d\theta}{dt} + \omega_0^2\theta = \frac{F}{mL}\cos(\omega_d t)
$$

### Resonance Condition
Maximum energy transfer occurs when:
$$
\omega_d = \sqrt{\omega_0^2 - 2\beta^2}
$$

## Dynamics Analysis

| Parameter          | Effect on System                  |
|--------------------|-----------------------------------|
| Damping (β) ↑      | Resonance peak broadens           |
| Driving Force (F) ↑| Amplitude increases               |
| Frequency Ratio (ω_d/ω_0) | Phase shifts occur        |

## Practical Applications
- **Energy Harvesting:** Tuning ω_d to match ω₀ for maximum power
- **Structural Engineering:** Avoiding resonance in bridges
- **Biological Systems:** Neural oscillations modeling

---

## Interactive Simulation

<div id="pendulumPlot"></div>

<div style="border: 1px solid #ddd; padding: 15px; margin: 20px 0;">
    <div class="param-control">
        <label>Damping (β): <input type="range" id="beta" min="0" max="1" step="0.01" value="0.1"></label>
        <label>Force (F): <input type="range" id="force" min="0" max="5" step="0.1" value="1.5"></label>
        <label>Frequency (ω_d): <input type="range" id="freq" min="0.5" max="3" step="0.1" value="1.0"></label>
    </div>
</div>

<script src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>
<script>
(function() {
    const solvePendulum = (beta, F, omega_d, tmax=30) => {
        const dt = 0.05;
        const omega0 = 1.0;
        let theta = 0.1, omega = 0;
        const solution = [];
        
        for(let t=0; t<=tmax; t+=dt) {
            const acceleration = F*Math.cos(omega_d*t) - 
                               2*beta*omega0*omega - 
                               omega0**2*Math.sin(theta);
            
            omega += acceleration * dt;
            theta += omega * dt;
            
            solution.push({
                t: t.toFixed(2),
                theta: theta,
                omega: omega
            });
        }
        return solution;
    };

    function updatePlot() {
        const beta = parseFloat(document.getElementById('beta').value);
        const F = parseFloat(document.getElementById('force').value);
        const omega_d = parseFloat(document.getElementById('freq').value);
        
        const data = solvePendulum(beta, F, omega_d);
        
        const trace1 = {
            x: data.map(d => d.t),
            y: data.map(d => d.theta),
            name: 'Angular Displacement',
            type: 'scatter'
        };
        
        const trace2 = {
            x: data.map(d => d.theta),
            y: data.map(d => d.omega),
            name: 'Phase Portrait',
            mode: 'lines',
            type: 'scatter'
        };

        Plotly.newPlot('pendulumPlot', [trace1, trace2], {
            title: `Forced Damped Pendulum Dynamics<br>β=${beta}, F=${F}, ω<sub>d</sub>=${omega_d}`,
            grid: {rows: 1, columns: 2},
            xaxis1: {title: 'Time (s)'},
            yaxis1: {title: 'θ (rad)'},
            xaxis2: {title: 'θ (rad)'},
            yaxis2: {title: 'dθ/dt (rad/s)'}
        });
    }

    document.querySelectorAll('input[type="range"]').forEach(input => {
        input.addEventListener('input', updatePlot);
    });

    updatePlot();
})();
</script>