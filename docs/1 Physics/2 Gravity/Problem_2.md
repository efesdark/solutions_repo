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
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<div style="margin-top: 30px;">
  <label for="planetSelector"><strong>Choose a planet:</strong></label>
  <select id="planetSelector">
    <option value="earth">🌍 Earth</option>
    <option value="mars">🔴 Mars</option>
    <option value="jupiter">🟤 Jupiter</option>
  </select>
  <canvas id="velocityChart" width="600" height="400"></canvas>
</div>

<script>
  const planets = {
    earth: {
      name: "Earth",
      mass: 5.972e24, // in kg
      radius: 6371e3  // in meters
    },
    mars: {
      name: "Mars",
      mass: 6.39e23,
      radius: 3389.5e3
    },
    jupiter: {
      name: "Jupiter",
      mass: 1.898e27,
      radius: 69911e3
    }
  };

  const G = 6.67430e-11; // Gravitational constant

  function calculateVelocities(planet) {
    const r = planet.radius;
    const M = planet.mass;

    const v1 = Math.sqrt(G * M / r);        // 1st Cosmic Velocity
    const v2 = Math.sqrt(2) * v1;           // 2nd Cosmic Velocity
    const v3 = Math.sqrt(G * 1.989e30 / (1.5e11)) + v2; // Approx. from Earth orbit + planet escape

    return [v1 / 1000, v2 / 1000, v3 / 1000]; // convert to km/s
  }

  const ctx = document.getElementById("velocityChart").getContext("2d");
  let velocityChart = null;

  function updateChart(planetKey) {
    const planet = planets[planetKey];
    const [v1, v2, v3] = calculateVelocities(planet);

    const data = {
      labels: [
        "1st Cosmic Velocity\n(Orbiting)", 
        "2nd Cosmic Velocity\n(Escape)", 
        "3rd Cosmic Velocity\n(Leaving Solar System)"
      ],
      datasets: [{
        label: `Velocities for ${planet.name} (km/s)`,
        data: [v1, v2, v3],
        backgroundColor: ['#007bff', '#28a745', '#ff5722']
      }]
    };

    const config = {
      type: 'bar',
      data: data,
      options: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: `Cosmic Velocities for ${planet.name}`
          },
          tooltip: {
            callbacks: {
              label: function(context) {
                return `${context.dataset.label}: ${context.parsed.y.toFixed(2)} km/s`;
              }
            }
          }
        },
        scales: {
          y: {
            beginAtZero: true,
            title: {
              display: true,
              text: 'Velocity (km/s)'
            }
          }
        }
      }
    };

    if (velocityChart) {
      velocityChart.destroy();
    }

    velocityChart = new Chart(ctx, config);
  }

  document.getElementById("planetSelector").addEventListener("change", (e) => {
    updateChart(e.target.value);
  });

  updateChart("earth"); // default chart
</script>
