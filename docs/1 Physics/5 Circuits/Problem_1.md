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

## JavaScript Simulation

Below is a simple interactive simulation to add resistors in series or parallel and compute the equivalent resistance.

```html
<style>
  body { font-family: Arial, sans-serif; max-width: 700px; margin: auto; padding: 20px;}
  input[type="number"] { width: 100px; margin-right: 10px;}
  button { margin: 5px; padding: 8px 12px; }
  .result { margin-top: 20px; font-weight: bold; font-size: 1.2em; }
  #resistor-list { margin-top: 10px; }
</style>

<h3>Add Resistors</h3>
<label>Resistance (Ω): <input type="number" id="resistanceInput" min="0.01" step="0.01" value="10"></label><br>
<button onclick="addSeries()">Add in Series</button>
<button onclick="addParallel()">Add in Parallel</button>
<button onclick="resetCircuit()">Reset</button>

<div id="resistor-list"></div>
<div class="result" id="result">Equivalent Resistance: 0 Ω</div>

<script>
  let resistors = [];

  function updateResult() {
    if (resistors.length === 0) {
      document.getElementById('result').innerText = 'Equivalent Resistance: 0 Ω';
      document.getElementById('resistor-list').innerText = '';
      return;
    }
    // Calculate equivalent resistance
    // Assume resistors array holds objects: { type: 'series'|'parallel', value: Number }

    // We process the resistors array step-by-step
    // Starting with the first resistor value:
    let Req = resistors[0].value;

    for (let i = 1; i < resistors.length; i++) {
      const r = resistors[i].value;
      const t = resistors[i].type;
      if (t === 'series') {
        Req += r; // Series: sum resistances
      } else if (t === 'parallel') {
        Req = 1 / (1/Req + 1/r); // Parallel: reciprocal sum
      }
    }

    document.getElementById('result').innerText = 'Equivalent Resistance: ' + Req.toFixed(3) + ' Ω';

    // Show the list of resistors added
    const listText = resistors.map((r, idx) => 
      `${idx === 0 ? 'Initial resistor' : (r.type === 'series' ? 'Series' : 'Parallel')}: ${r.value} Ω`
    ).join('\n');
    document.getElementById('resistor-list').innerText = listText;
  }

  function addSeries() {
    const input = document.getElementById('resistanceInput');
    const val = parseFloat(input.value);
    if (isNaN(val) || val <= 0) {
      alert('Please enter a positive resistance value.');
      return;
    }
    if (resistors.length === 0) {
      resistors.push({ type: 'series', value: val }); // First resistor
    } else {
      resistors.push({ type: 'series', value: val });
    }
    updateResult();
  }

  function addParallel() {
    const input = document.getElementById('resistanceInput');
    const val = parseFloat(input.value);
    if (isNaN(val) || val <= 0) {
      alert('Please enter a positive resistance value.');
      return;
    }
    if (resistors.length === 0) {
      resistors.push({ type: 'parallel', value: val }); // First resistor
    } else {
      resistors.push({ type: 'parallel', value: val });
    }
    updateResult();
  }

  function resetCircuit() {
    resistors = [];
    updateResult();
  }

  // Initialize
  updateResult();
</script>
