# Problem 1: Interference Patterns on a Water Surface

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

## Computational Tool

```html
<div style="margin-top: 30px;">
  <label for="polygonSidesInput"><strong>Polygon Sides (e.g., 3 for Triangle, 4 for Square):</strong></label>
  <input type="number" id="polygonSidesInput" value="4" step="1" min="3" max="10">
  <label for="waveAmplitudeInput"><strong>Wave Amplitude:</strong></label>
  <input type="number" id="waveAmplitudeInput" value="1" step="0.1" min="0.1" max="5">
  <canvas id="interferenceChart" width="300" height="300"></canvas>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  const A = 1;  // Amplitude of the wave
  const k = 2 * Math.PI / 10;  // Example wavelength
  const omega = 2 * Math.PI * 1;  // Example frequency

  // Function to calculate wave displacement at a point (x, y)
  function calculateWave(x, y, sources, t) {
    let displacement = 0;
    sources.forEach(source => {
      let dx = x - source.x;
      let dy = y - source.y;
      let r = Math.sqrt(dx * dx + dy * dy);  // Distance from source to point
      displacement += A * Math.sin(k * r - omega * t);  // Superposition of waves
    });
    return displacement;
  }

  function generateInterferencePattern(sides, amplitude, t) {
    const width = 300;
    const height = 300;
    const centerX = width / 2;
    const centerY = height / 2;
    const radius = 100;
    const angleStep = 2 * Math.PI / sides;

    let sources = [];
    for (let i = 0; i < sides; i++) {
      let angle = i * angleStep;
      let x = centerX + radius * Math.cos(angle);
      let y = centerY + radius * Math.sin(angle);
      sources.push({ x, y });
    }

    let data = [];
    const gridSize = 30;  // Reducing the grid size further
    for (let x = 0; x < width; x += Math.floor(width / gridSize)) {
      for (let y = 0; y < height; y += Math.floor(height / gridSize)) {
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

    // Simplified heatmap data
    const data = {
      labels: patternData.map(point => point.x),
      datasets: [{
        label: `Interference Pattern (Polygon with ${sides} sides)`,
        data: patternData.map(point => ({ x: point.x, y: point.y, r: point.displacement })),
        backgroundColor: patternData.map(point => `rgba(0, 0, 255, ${Math.abs(point.displacement) / 5})`), // Lower alpha for smoother transition
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
