
## Problem 1  
### Exploring the Central Limit Theorem through Simulations

---

### Motivation

The **Central Limit Theorem (CLT)** is a fundamental result in statistics which states:

> *The distribution of the sample mean approaches a normal distribution as the sample size \( n \) increases, regardless of the original population distribution.*

This theorem is crucial because it justifies why the normal distribution is commonly used in inferential statistics even when the underlying population is not normal.

---

### Theoretical Background

Given a population with mean \( \mu \) and variance \( \sigma^2 \), and a sample size \( n \), the sampling distribution of the sample mean \( \bar{X} \) has:

- Mean: \( \mu_{\bar{X}} = \mu \)
- Variance: \( \sigma^2_{\bar{X}} = \frac{\sigma^2}{n} \)

As \( n \to \infty \), the distribution of \( \bar{X} \) approaches

\[
\bar{X} \sim \mathcal{N}\left(\mu, \frac{\sigma^2}{n}\right)
\]

regardless of the population distribution shape.

---

### Task & Simulation Setup

**Population Distributions:**

1. Uniform distribution \( U(a,b) \)
2. Exponential distribution with rate \( \lambda \)
3. Binomial distribution \( \text{Binomial}(n, p) \)

**Sampling:**

- For each distribution, generate a large population (e.g., 10,000 samples).
- Randomly draw samples of size \( n = 5, 10, 30, 50 \).
- Compute sample means.
- Repeat sampling multiple times (e.g., 1000 repetitions) to build the sampling distribution of the mean.
- Visualize histograms of sample means.

---

### Interactive Simulation 

<div>
  <label>Sample Size: <input type="number" id="sampleSize" min="1" value="30"></label>
  <label>Repetitions: <input type="number" id="repetitions" min="100" value="1000"></label>
  <select id="distributionSelect">
    <option value="uniform">Uniform (0,1)</option>
    <option value="exponential">Exponential (λ=1)</option>
    <option value="binomial">Binomial (n=10, p=0.5)</option>
  </select>
  <button onclick="runCLTSimulation()">Run Simulation</button>
</div>

<canvas id="cltCanvas" width="600" height="400" style="border:1px solid #ccc;margin-top:10px;"></canvas>

<script>
  function getRandomUniform() {
    return Math.random();
  }

  function getRandomExponential(lambda=1) {
    return -Math.log(1 - Math.random()) / lambda;
  }

  function getRandomBinomial(n=10, p=0.5) {
    let success = 0;
    for(let i=0; i<n; i++) {
      if (Math.random() < p) success++;
    }
    return success;
  }

  function runCLTSimulation() {
    const sampleSize = parseInt(document.getElementById('sampleSize').value);
    const repetitions = parseInt(document.getElementById('repetitions').value);
    const dist = document.getElementById('distributionSelect').value;

    let populationFunc;
    switch(dist) {
      case 'uniform': populationFunc = getRandomUniform; break;
      case 'exponential': populationFunc = () => getRandomExponential(1); break;
      case 'binomial': populationFunc = () => getRandomBinomial(10, 0.5); break;
    }

    let sampleMeans = [];
    for(let i=0; i<repetitions; i++) {
      let sum = 0;
      for(let j=0; j<sampleSize; j++) {
        sum += populationFunc();
      }
      sampleMeans.push(sum / sampleSize);
    }

    drawHistogram(sampleMeans, sampleSize, dist);
  }

  function drawHistogram(data, sampleSize, dist) {
    const canvas = document.getElementById('cltCanvas');
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const bins = 40;
    const min = Math.min(...data);
    const max = Math.max(...data);
    const binWidth = (max - min) / bins;
    let histogram = new Array(bins).fill(0);

    data.forEach(d => {
      const idx = Math.min(bins - 1, Math.floor((d - min) / binWidth));
      histogram[idx]++;
    });

    const maxCount = Math.max(...histogram);

    // Draw histogram
    const margin = 40;
    const graphWidth = canvas.width - 2*margin;
    const graphHeight = canvas.height - 2*margin;

    ctx.fillStyle = '#007bff';
    for(let i=0; i<bins; i++) {
      const x = margin + i * (graphWidth / bins);
      const height = (histogram[i] / maxCount) * graphHeight;
      ctx.fillRect(x, margin + graphHeight - height, graphWidth / bins - 2, height);
    }

    // Axis
    ctx.strokeStyle = 'black';
    ctx.beginPath();
    ctx.moveTo(margin, margin);
    ctx.lineTo(margin, margin + graphHeight);
    ctx.lineTo(margin + graphWidth, margin + graphHeight);
    ctx.stroke();

    // Labels
    ctx.fillStyle = 'black';
    ctx.font = '14px Arial';
    ctx.fillText(`CLT: Sample Size=${sampleSize}, Distribution=${dist}`, margin, margin - 10);
    ctx.fillText(min.toFixed(2), margin, margin + graphHeight + 15);
    ctx.fillText(max.toFixed(2), margin + graphWidth - 30, margin + graphHeight + 15);
  }
</script>


