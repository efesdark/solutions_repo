# Problem 1


## Motivation
Interference occurs when waves from different sources overlap, creating new patterns. On a water surface, this can be easily observed when ripples from different points meet, forming distinctive interference patterns. These patterns can show us how waves combine in different ways, either reinforcing each other or canceling out.

Studying these patterns helps us understand wave behavior in a simple, visual way. It also allows us to explore important concepts, like the relationship between wave phase and the effects of multiple sources. This task offers a hands-on approach to learning about wave interactions and their real-world applications, making it an interesting and engaging way to dive into wave physics.

## Task
- **Select a Regular Polygon:** Choose a regular polygon (e.g., equilateral triangle, square, regular pentagon).
- **Position the Sources:** Place point wave sources at the vertices of the selected polygon.
- **Wave Equations:** Write the equations describing the waves emitted from each source, considering their respective positions.
- **Superposition of Waves:** Apply the principle of superposition by summing the wave displacements at each point on the water surface.
- **Analyze Interference Patterns:** Examine the resulting displacement as a function of position and time. Identify regions of constructive interference (wave amplification) and destructive interference (wave cancellation).
- **Visualization:** Present your findings graphically, illustrating the interference patterns for the chosen regular polygon.

## Wave Equation

The displacement of the water surface at a point \( (x, y) \) and time \( t \) due to a single wave emitted from a point source is described by the following equation:

\[
\zeta(x, y, t) = A \sin(k r - \omega t + \phi_0)
\]

Where:
- \( \zeta(x, y, t) \) is the displacement at point \( (x, y) \) at time \( t \),
- \( A \) is the amplitude of the wave,
- \( k \) is the wave number, related to the wavelength \( \lambda \) by \( k = \frac{2\pi}{\lambda} \),
- \( \omega \) is the angular frequency, related to the frequency \( f \) by \( \omega = 2 \pi f \),
- \( r \) is the distance from the source to the point \( (x, y) \),
- \( \phi_0 \) is the initial phase.

The superposition of waves from multiple sources is given by the sum of the individual displacements:

\[
\zeta(x, y, t) = \sum_{i=1}^{N} A_i \sin(k r_i - \omega t + \phi_{0i})
\]

Where:
- \( N \) is the number of sources (vertices of the polygon),
- \( A_i \) is the amplitude of the wave from source \( i \),
- \( r_i \) is the distance from the source \( i \) to the point \( (x, y) \),
- \( \phi_{0i} \) is the initial phase of the wave from source \( i \).

## Interference Patterns
By applying the principle of superposition, we can visualize the interference patterns formed by multiple point sources. The result is a pattern of constructive and destructive interference:
- **Constructive Interference** occurs when the waves from different sources reinforce each other, leading to larger displacements.
- **Destructive Interference** occurs when the waves from different sources cancel each other out, leading to smaller or zero displacement.

This results in a characteristic interference pattern on the water surface that can be analyzed and visualized graphically.

## Computational Tool
The following code simulates the interference patterns on a water surface due to the superposition of waves from multiple sources placed at the vertices of a regular polygon. The simulation will visualize the water surface showing regions of constructive and destructive interference.

---

### Interactive Simulation:

<div style="margin-top: 30px;">
  <label for="polygonSidesInput"><strong>Number of Polygon Sides (e.g., 3 for Triangle, 4 for Square):</strong></label>
  <input type="number" id="polygonSidesInput" value="4" step="1" min="3" max="10">
  <label for="waveAmplitudeInput"><strong>Wave Amplitude:</strong></label>
  <input type="number" id="waveAmplitudeInput" value="1" step="0.1" min="0.1" max="5">
  <canvas id="interferenceChart" width="600" height="400"></canvas>
</div>

<!-- Load Chart.js Library -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  // Constants for the wave
  const A = 1;  // Amplitude of the wave
  const k = 2 * Math.PI / 10;  // Example wavelength (arbitrary, can be adjusted)
  const omega = 2 * Math.PI * 1;  // Example frequency (1 Hz for simplicity)

  // Function to calculate wave displacement at a point (x, y)
  function calculateWave(x, y, sources, t) {
    let displacement = 0;
    sources.forEach(source => {
      let dx = x - source.x;
      let dy = y - source.y;
      let r = Math.sqrt(dx * dx + dy * dy);  // Distance from source to point (x, y)
      displacement += A * Math.sin(k * r - omega * t);  // Superposition of waves
    });
    return displacement;
  }

  // Function to generate interference pattern
  function generateInterferencePattern(sides, amplitude, t) {
    const width = 600;
    const height = 400;
    const centerX = width / 2;
    const centerY = height / 2;
    const radius = 150;  // Radius of the polygon
    const angleStep = 2 * Math.PI / sides;  // Angle between sources

    // Generate sources at the vertices of the polygon
    let sources = [];
    for (let i = 0; i < sides; i++) {
      let angle = i * angleStep;
      let x = centerX + radius * Math.cos(angle);
      let y = centerY + radius * Math.sin(angle);
      sources.push({ x, y });
    }

    // Create the data for the interference pattern
    let data = [];
    for (let x = 0; x < width; x++) {
      for (let y = 0; y < height; y++) {
        let displacement = calculateWave(x, y, sources, t);
        data.push({ x: x, y: y, displacement: displacement });
      }
    }

    return data;
  }

  const ctx = document.getElementById("interferenceChart").getContext("2d");
  let interferenceChart = null;

  function updateChart() {
    const sides = parseInt(document.getElementById("polygonSidesInput").value);
    const amplitude = parseFloat(document.getElementById("waveAmplitudeInput").value);
    const t = Date.now() / 1000;  // Use current time as the time variable

    const patternData = generateInterferencePattern(sides, amplitude, t);

    // Create heatmap-like data for the chart
    const data = {
      labels: patternData.map(point => point.x),
      datasets: [{
        label: `Interference Pattern (Polygon with ${sides} sides)`,
        data: patternData.map(point => ({ x: point.x, y: point.y, r: point.displacement })),
        backgroundColor: patternData.map(point => `rgba(0, 0, 255, ${Math.abs(point.displacement)})`),
        borderWidth: 0
      }]
    };

    const config = {
      type: 'scatter',
      data: data,
      options: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Water Surface Interference Pattern'
          }
        },
        scales: {
          x: {
            display: false
          },
          y: {
            display: false
          }
        }
      }
    };

    if (interferenceChart) {
      interferenceChart.destroy();
    }

    interferenceChart = new Chart(ctx, config);
  }

  document.getElementById("polygonSidesInput").addEventListener("input", updateChart);
  document.getElementById("waveAmplitudeInput").addEventListener("input", updateChart);

  updateChart(); // Initial chart
</script>
