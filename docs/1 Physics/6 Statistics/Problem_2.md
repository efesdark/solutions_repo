# Problem 2: Estimating π using Monte Carlo Simulations

## Objective

This report explores two Monte Carlo simulation methods to estimate the mathematical constant \( \pi \):

1. **Geometric Sampling with a Unit Circle**
2. **Buffon's Needle Experiment**

Each method includes theoretical background, JavaScript-based simulation, and observations regarding accuracy and convergence.

---

## Method 1: Estimating \( \pi \) via Unit Circle Sampling

### Theoretical Background

We use a square of side length 2, centered at the origin, with an inscribed circle of radius 1.

- Area of the circle: \( A_{\text{circle}} = \pi r^2 = \pi \)
- Area of the square: \( A_{\text{square}} = 4 \)

The probability of a random point falling inside the circle is:

\[
\frac{\pi}{4} \quad \Rightarrow \quad \pi \approx 4 \times \frac{\text{Points in circle}}{\text{Total points}}
\]

### JavaScript Simulation

```html
<!DOCTYPE html>
<html>
<head>
  <title>Monte Carlo Pi Estimation - Circle</title>
  <style>
    canvas { border: 1px solid #ccc; }
    body { font-family: sans-serif; }
  </style>
</head>
<body>
  <h2>Monte Carlo Simulation: Estimating π (Circle Method)</h2>
  <label>Number of Points: <input type="number" id="numPoints" value="5000" min="100"></label>
  <button onclick="estimatePi()">Run Simulation</button>
  <p id="piResult"></p>
  <canvas id="canvas" width="400" height="400"></canvas>

  <script>
    function estimatePi() {
      const canvas = document.getElementById('canvas');
      const ctx = canvas.getContext('2d');
      const N = parseInt(document.getElementById('numPoints').value);
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      let inside = 0;

      for (let i = 0; i < N; i++) {
        let x = Math.random() * 2 - 1;
        let y = Math.random() * 2 - 1;
        let px = (x + 1) * canvas.width / 2;
        let py = (y + 1) * canvas.height / 2;

        if (x * x + y * y <= 1) {
          ctx.fillStyle = 'green';
          inside++;
        } else {
          ctx.fillStyle = 'red';
        }
        ctx.fillRect(px, py, 1, 1);
      }

      const piEstimate = 4 * inside / N;
      document.getElementById('piResult').innerText = `Estimated π = ${piEstimate.toFixed(6)}`;
    }
  </script>
</body>
</html>
