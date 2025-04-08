# Problem 2

## Motivation

The concept of escape velocity is crucial for understanding the conditions required to leave a celestial body's gravitational influence. Extending this concept, the first, second, and third cosmic velocities define the thresholds for orbiting, escaping, and leaving a star system.

These principles underpin modern space exploration, from launching satellites to interplanetary missions.

---

## Definitions

- **First Cosmic Velocity**  
  Circular orbital velocity near the planet’s surface:  
  $v_1 = \sqrt{\frac{GM}{R}}$

- **Second Cosmic Velocity**  
  Escape velocity from the planet's gravity:  
  $v_2 = \sqrt{2} \cdot v_1$

- **Third Cosmic Velocity**  
  Escape velocity from the solar system:  
  $v_3 = \sqrt{\frac{2GM_\odot}{r}}$

---

## Application

- **Satellites**: Require first cosmic velocity to stay in orbit.
- **Moon and Mars Missions**: Require second cosmic velocity.
- **Interstellar Probes**: Must reach the third cosmic velocity.

---

## Interactive Chart

Select a planet to see its cosmic velocities:

<div id="velocityChart" style="width: 100%; height: 500px;"></div>

<select id="planetSelector">
  <option value="earth">Earth</option>
  <option value="mars"> Mars</option>
  <option value="jupiter">Jupiter</option>
</select>

<script src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>
<script>
  window.addEventListener('DOMContentLoaded', function () {
    const G = 6.67430e-11;
    const bodies = {
      earth: { name: "Earth", mass: 5.972e24, radius: 6371e3, distance: 1.496e11 },
      mars: { name: "Mars", mass: 6.417e23, radius: 3389.5e3, distance: 2.279e11 },
      jupiter: { name: "Jupiter", mass: 1.898e27, radius: 69911e3, distance: 7.785e11 }
    };

    const sunMass = 1.989e30;

    const plotDiv = document.getElementById('velocityChart');
    const selector = document.getElementById('planetSelector');

    function updateChart(planetKey) {
      const body = bodies[planetKey];
      const v1 = Math.sqrt(G * body.mass / body.radius);
      const v2 = Math.sqrt(2 * G * body.mass / body.radius);
      const v3 = Math.sqrt(2 * G * sunMass / body.distance);

      const velocities = [v1, v2, v3].map(v => v / 1000); // Convert to km/s

      Plotly.newPlot(plotDiv, [{
        x: ['1st Cosmic', '2nd Cosmic', '3rd Cosmic'],
        y: velocities,
        type: 'bar',
        text: velocities.map(v => v.toFixed(2) + ' km/s'),
        textposition: 'auto',
        marker: { color: ['#1f77b4', '#ff7f0e', '#2ca02c'] }
      }], {
        title: `${body.name} Cosmic Velocities`,
        yaxis: { title: 'Velocity (km/s)' }
      });
    }

    selector.addEventListener('change', () => updateChart(selector.value));
    updateChart('earth');
  });
</script>
