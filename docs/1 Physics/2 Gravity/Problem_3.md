# Problem 3

## Motivation
When an object is released from a moving rocket near Earth, its trajectory depends on initial conditions and gravitational forces. This scenario presents a rich problem, blending principles of orbital mechanics and numerical methods. Understanding the potential trajectories is vital for space missions, such as deploying payloads or returning objects to Earth.

## Task
- **Analyze the possible trajectories** (e.g., parabolic, hyperbolic, elliptical) of a payload released near Earth.
- Perform a **numerical analysis** to compute the path of the payload based on given initial conditions (position, velocity, and altitude).
- Discuss how these trajectories relate to **orbital insertion**, **reentry**, or **escape** scenarios.
- Develop a **computational tool** to simulate and visualize the motion of the payload under Earth's gravity, accounting for initial velocities and directions.

## Gravitational Principles
To understand the behavior of a freely released payload near Earth, we must rely on **Newton's Law of Gravitation** and the principles of **orbital mechanics**:

- The gravitational force between Earth and the payload is inversely proportional to the square of the distance between their centers.
- The object’s trajectory will depend on its initial velocity and the direction of release.

### Equations Used:
Newton’s Law of Gravitation:
$$
F = \frac{GMm}{r^2}
$$
Where:
- \( G \) is the gravitational constant.
- \( M \) is the mass of Earth.
- \( m \) is the mass of the payload.
- \( r \) is the distance between the center of the Earth and the payload.

The trajectory can be computed using basic kinematic equations, updated step-by-step via numerical methods.

## Trajectories Types:
1. **Parabolic Trajectory:** Occurs when the payload is released with a velocity below the escape velocity but at an angle.
2. **Hyperbolic Trajectory:** Occurs when the payload’s velocity exceeds the escape velocity.
3. **Elliptical Trajectory:** Occurs when the payload’s velocity is sufficient to form a closed orbit around Earth.

### Real-World Applications
- **Space Mission Planning:** Understanding payload trajectories is essential for deploying satellites or performing rendezvous operations.
- **Reentry Scenarios:** Calculating the correct reentry trajectory ensures safe return of payloads, such as space capsules.
- **Escape Scenarios:** Understanding escape velocities is crucial for interplanetary missions and space exploration.

## Computational Tool
The following code simulates the trajectory of a freely released payload near Earth, taking into account the initial velocity and launch angle. The simulation provides a visual representation of the trajectory, illustrating how the payload will behave under the influence of gravity.

---

### Interactive Simulation:

<div style="margin-top: 30px;">
  <label for="velocityInput"><strong>Initial Velocity (km/s):</strong></label>
  <input type="number" id="velocityInput" value="8.0" step="0.1" min="0" max="20">
  <label for="angleInput"><strong>Launch Angle (°):</strong></label>
  <input type="number" id="angleInput" value="45" step="1" min="0" max="90">
  <canvas id="trajectoryChart" width="600" height="400"></canvas>
</div>

<script>
  const G = 6.67430e-11; // Gravitational constant
  const M = 5.972e24; // Earth mass in kg
  const R = 6371e3; // Earth radius in meters
  const dt = 0.05; // time step in seconds

  function calculateTrajectory(initialVelocity, launchAngle, tMax = 5000) {
    let x = 0;
    let y = R + 100000; // 100 km altitude
    let vx = initialVelocity * Math.cos(launchAngle * Math.PI / 180);
    let vy = initialVelocity * Math.sin(launchAngle * Math.PI / 180);
    let trajectory = [];

    for (let t = 0; t < tMax; t += dt) {
      let r = Math.sqrt(x * x + y * y); // distance from Earth's center
      let ax = -G * M * x / (r * r * r); // gravitational acceleration in x
      let ay = -G * M * y / (r * r * r); // gravitational acceleration in y

      vx += ax * dt;
      vy += ay * dt;
      x += vx * dt;
      y += vy * dt;

      trajectory.push({ x: x / 1000, y: y / 1000 }); // Convert to km
      if (y <= R) break; // Stop if it hits the Earth's surface
    }
    return trajectory;
  }

  const ctx = document.getElementById("trajectoryChart").getContext("2d");
  let trajectoryChart = null;

  function updateChart() {
    const initialVelocity = parseFloat(document.getElementById("velocityInput").value);
    const launchAngle = parseFloat(document.getElementById("angleInput").value);
    const trajectory = calculateTrajectory(initialVelocity, launchAngle);

    const data = {
      labels: trajectory.map((point, idx) => idx * dt),
      datasets: [{
        label: `Trajectory (Initial Velocity: ${initialVelocity} km/s, Angle: ${launchAngle}°)`,
        data: trajectory.map(point => ({ x: point.x, y: point.y })),
        borderColor: '#007bff',
        fill: false,
        tension: 0.1
      }]
    };

    const config = {
      type: 'line',
      data: data,
      options: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Payload Trajectory Near Earth'
          }
        },
        scales: {
          x: {
            title: {
              display: true,
              text: 'Distance (km)'
            }
          },
          y: {
            title: {
              display: true,
              text: 'Altitude (km)'
            }
          }
        }
      }
    };

    if (trajectoryChart) {
      trajectoryChart.destroy();
    }

    trajectoryChart = new Chart(ctx, config);
  }

  document.getElementById("velocityInput").addEventListener("input", updateChart);
  document.getElementById("angleInput").addEventListener("input", updateChart);

  updateChart(); // Initial chart
</script>
