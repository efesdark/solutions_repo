# Problem 1

## Theoretical Foundation
From Newton's Law of Gravitation and centripetal force equivalence:
$$
\frac{GMm}{r^2} = \frac{mv^2}{r}
$$
Substituting orbital velocity $v = \frac{2\pi r}{T}$, we derive:
$$
T^2 = \left(\frac{4\pi^2}{GM}\right)r^3
$$

## Astronomical Significance
| Application               | Implementation                  |
|---------------------------|---------------------------------|
| Exoplanet Detection       | Mass estimation via period analysis |
| Satellite Engineering     | Geostationary orbit calculation |
| Galactic Dynamics         | Dark matter distribution studies |

---

## Interactive Simulation

<div id="orbitPlot"></div>
<div id="lawVerification"></div>

<div class="control-panel">
    <div class="param">
        <label>Orbital Radius (Earth Radii): 
            <input type="range" id="radius" min="2" max="100" value="60" step="1">
            <span id="radiusValue">60</span>
        </label>
    </div>
    <div class="param">
        <label>Central Mass (Earth Masses): 
            <input type="range" id="mass" min="0.1" max="10" value="1" step="0.1">
            <span id="massValue">1</span>
        </label>
    </div>
</div>

<script src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>
<script>
(function() {
    const G = 6.67430e-11;  // Gravitational constant
    const earthMass = 5.972e24;  // kg
    const earthRadius = 6.371e6;  // meters

    function calculatePeriod(r, M) {
        return Math.sqrt((4 * Math.PI**2 * r**3) / (G * M));
    }

    function updateSimulation() {
        const radius = parseFloat(document.getElementById('radius').value);
        const mass = parseFloat(document.getElementById('mass').value);
        
        // Convert to SI units
        const rSI = radius * earthRadius;
        const MSI = mass * earthMass;
        
        // Calculate orbital period
        const T = calculatePeriod(rSI, MSI);
        
        // Generate orbit path
        const theta = Array.from({length: 100}, (_, i) => i * 2 * Math.PI / 100);
        const x = radius * theta.map(t => Math.cos(t));
        const y = radius * theta.map(t => Math.sin(t));
        
        // Update orbit plot
        Plotly.newPlot('orbitPlot', [{
            x: x,
            y: y,
            mode: 'lines',
            name: 'Orbit',
            line: {color: '#00ff88'}
        }, {
            x: [0],
            y: [0],
            mode: 'markers',
            marker: {size: 20, color: '#1e90ff'},
            name: 'Central Body'
        }], {
            title: `Orbital Period: ${(T/3600).toFixed(2)} hours`,
            showlegend: false,
            aspectratio: {x: 1, y: 1}
        });

        // Generate verification data
        const radii = Array.from({length: 50}, (_, i) => 2 + (i*2));
        const periods = radii.map(r => 
            calculatePeriod(r * earthRadius, MSI) / 3600
        );
        
        // Update law verification plot
        Plotly.newPlot('lawVerification', [{
            x: radii.map(r => r**3),
            y: periods.map(t => t**2),
            mode: 'lines+markers',
            name: 'T² vs r³'
        }], {
            title: 'Kepler\'s Third Law Verification',
            xaxis: {title: 'Orbital Radius³ (R⊕³)'},
            yaxis: {title: 'Orbital Period² (hours²)'}
        });
    }

    // Add event listeners
    document.querySelectorAll('input[type="range"]').forEach(input => {
        input.addEventListener('input', function() {
            document.getElementById(this.id + 'Value').textContent = this.value;
            updateSimulation();
        });
    });

    // Initial render
    updateSimulation();
})();
</script>

---

## Practical Applications

### Satellite Deployment
$$
r_{geo} = \left(\frac{T^2GM}{4\pi^2}\right)^{1/3}
$$
Used to calculate geostationary orbit at ~42,164 km from Earth center

### Exoplanet Characterization
Radial velocity method uses period measurements to estimate:
$$
M \propto \frac{r^3}{T^2}
$$

### Historical Navigation
Ancient mariners used Moon orbital period (sidereal month = 27.3 days) for tidal predictions