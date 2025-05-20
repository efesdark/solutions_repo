# Circuits

## Problem 1  
### Equivalent Resistance Using Graph Theory

---

## Motivation

Calculating equivalent resistance is a fundamental problem in electrical circuits, essential for understanding and designing efficient systems. While traditional methods involve iteratively applying series and parallel resistor rules, these approaches become cumbersome for complex circuits. Graph theory offers a structured and algorithmic way to analyze circuits by representing them as graphs, where nodes correspond to junctions and edges represent resistors.

This approach simplifies calculations and supports automation, making it highly valuable in circuit simulation and design.

---

## Fundamental Concepts and Formulas

### Series Resistors

Resistors connected end-to-end, carrying the same current. Equivalent resistance:

$$
R_{eq} = R_1 + R_2 + \cdots + R_n
$$

### Parallel Resistors

Resistors connected across the same two nodes, sharing voltage. Equivalent resistance:

$$
\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}
$$

or equivalently:

$$
R_{eq} = \left( \sum_{i=1}^n \frac{1}{R_i} \right)^{-1}
$$

---

## Graph Theory Approach

Representing circuits as graphs with nodes and weighted edges (resistors) allows systematic reduction:

- **Series reduction:** Combine resistors in series by summing resistances.
- **Parallel reduction:** Combine parallel resistors via reciprocal sum.

Iterate until a single equivalent resistance remains between the input and output nodes.

---

## Interactive Circuit Simulator

Add resistors in series or parallel, edit their values, and see the circuit visually and the equivalent resistance calculated in real-time.

---

<style>
  body { font-family: Arial, sans-serif; max-width: 700px; margin: auto; padding: 20px;}
  input[type="number"] { width: 80px; margin-right: 10px;}
  button { margin: 5px 8px 5px 0; padding: 6px 12px; cursor: pointer; }
  #resistor-list { margin-top: 15px; white-space: pre-wrap; font-family: monospace; }
  #canvas-container { text-align: center; margin-top: 20px; }
  #circuitCanvas { border: 1px solid #ccc; background: #f9f9f9; }
  .resistor-label { font-weight: bold; font-size: 12px; }
</style>

<div>
  <label>Resistance (Ω): <input type="number" id="resistanceInput" min="0.01" step="0.01" value="10"></label><br>
  <button onclick="addSeries()">Add Resistor in Series</button>
  <button onclick="addParallel()">Add Resistor in Parallel</button>
  <button onclick="resetCircuit()">Reset Circuit</button>
</div>

<div id="resistor-list"></div>
<div class="result" id="result" style="margin-top:20px; font-weight:bold; font-size:1.3em;">Equivalent Resistance: 0 Ω</div>

<div id="canvas-container">
  <canvas id="circuitCanvas" width="680" height="180"></canvas>
</div>

<script>
  // Global variables
  let circuit = {
    type: 'series', // 'series' or 'parallel'
    resistors: []
  };

  // Resistor block size for drawing
  const resistorWidth = 60;
  const resistorHeight = 30;
  const spacing = 20;

  // Add resistor in series
  function addSeries() {
    const val = getResistanceInput();
    if (val === null) return;
    if (circuit.resistors.length === 0) {
      circuit.type = 'series';
    } else if (circuit.type !== 'series') {
      alert('Cannot mix series and parallel at this level. Reset and try again.');
      return;
    }
    circuit.resistors.push({ resistance: val });
    updateCircuit();
  }

  // Add resistor in parallel
  function addParallel() {
    const val = getResistanceInput();
    if (val === null) return;
    if (circuit.resistors.length === 0) {
      circuit.type = 'parallel';
    } else if (circuit.type !== 'parallel') {
      alert('Cannot mix parallel and series at this level. Reset and try again.');
      return;
    }
    circuit.resistors.push({ resistance: val });
    updateCircuit();
  }

  // Reset circuit
  function resetCircuit() {
    circuit = { type: 'series', resistors: [] };
    updateCircuit();
  }

  // Get and validate input resistance
  function getResistanceInput() {
    const input = document.getElementById('resistanceInput');
    const val = parseFloat(input.value);
    if (isNaN(val) || val <= 0) {
      alert('Please enter a positive resistance value.');
      return null;
    }
    return val;
  }

  // Calculate equivalent resistance recursively
  function calculateEquivalentResistance(circuit) {
    if (!circuit.resistors || circuit.resistors.length === 0) return 0;

    if (circuit.type === 'series') {
      return circuit.resistors.reduce((sum, r) => sum + r.resistance, 0);
    } else if (circuit.type === 'parallel') {
      const invSum = circuit.resistors.reduce((sum, r) => sum + 1/r.resistance, 0);
      return 1 / invSum;
    }
    return 0;
  }

  // Draw the circuit on canvas
  function drawCircuit() {
    const canvas = document.getElementById('circuitCanvas');
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    if (circuit.resistors.length === 0) {
      ctx.font = '16px Arial';
      ctx.fillText('No resistors added yet.', 20, canvas.height / 2);
      return;
    }

    ctx.strokeStyle = '#333';
    ctx.lineWidth = 2;
    ctx.font = '14px Arial';

    if (circuit.type === 'series') {
      // Draw series resistors horizontally
      let x = 20, y = canvas.height / 2;
      // Draw start line
      ctx.beginPath();
      ctx.moveTo(x - 10, y);
      ctx.lineTo(x, y);
      ctx.stroke();

      circuit.resistors.forEach((r, i) => {
        // Draw resistor rectangle
        ctx.strokeRect(x, y - resistorHeight/2, resistorWidth, resistorHeight);

        // Draw resistor label
        ctx.fillText(`R${i+1}: ${r.resistance.toFixed(2)} Ω`, x + 5, y + 5);

        // Draw wires
        ctx.beginPath();
        ctx.moveTo(x + resistorWidth, y);
        x += resistorWidth + spacing;
        ctx.lineTo(x, y);
        ctx.stroke();
      });

      // Draw end line
      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x + 10, y);
      ctx.stroke();

    } else if (circuit.type === 'parallel') {
      // Draw parallel resistors vertically stacked
      let x = canvas.width / 2;
      let startY = 20;
      const totalHeight = circuit.resistors.length * (resistorHeight + spacing) - spacing;

      // Draw vertical main lines
      ctx.beginPath();
      ctx.moveTo(x - 50, startY);
      ctx.lineTo(x - 50, startY + totalHeight);
      ctx.moveTo(x + 50, startY);
      ctx.lineTo(x + 50, startY + totalHeight);
      ctx.stroke();

      circuit.resistors.forEach((r, i) => {
        let y = startY + i * (resistorHeight + spacing);

        // Draw horizontal connecting wires to resistor
        ctx.beginPath();
        ctx.moveTo(x - 50, y + resistorHeight/2);
        ctx.lineTo(x - 10, y + resistorHeight/2);
        ctx.moveTo(x + 10, y + resistorHeight/2);
        ctx.lineTo(x + 50, y + resistorHeight/2);
        ctx.stroke();

        // Draw resistor rectangle
        ctx.strokeRect(x - 10, y, resistorWidth, resistorHeight);

        // Draw resistor label
        ctx.fillText(`R${i+1}: ${r.resistance.toFixed(2)} Ω`, x - 5, y + resistorHeight/1.7);
      });
    }
  }

  // Update circuit UI and results
  function updateCircuit() {
    drawCircuit();

    const Req = calculateEquivalentResistance(circuit);
    document.getElementById('result').innerText = `Equivalent Resistance: ${Req.toFixed(3)} Ω`;

    let listText = `Circuit type: ${circuit.type.toUpperCase()}\nResistors:\n`;
    circuit.resistors.forEach((r, i) => {
      listText += `  R${i+1}: ${r.resistance.toFixed(3)} Ω\n`;
    });
    document.getElementById('resistor-list').innerText = listText;
  }

  // Initialize
  resetCircuit();
</script>
