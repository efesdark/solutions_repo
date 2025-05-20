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
  input[type="number"] { width: 80px; margin-right: 10px;}
  button { margin: 5px 8px 5px 0; padding: 6px 12px; cursor: pointer; }
  #resistor-list { margin-top: 15px; white-space: pre-wrap; font-family: monospace; }
  #canvas-container { text-align: center; margin-top: 20px; }
  #circuitCanvas { border: 1px solid #ccc; background: #f9f9f9; }
</style>

<div>
  <label>Resistance (Ω): <input type="number" id="resistanceInput" min="0.01" step="0.01" value="10"></label><br>

  <button onclick="addResistor()">Add Resistor</button>
  <button onclick="startSeries()">Start Series</button>
  <button onclick="startParallel()">Start Parallel</button>
  <button onclick="endGroup()">End Group</button>
  <button onclick="resetCircuit()">Reset Circuit</button>
</div>

<div id="resistor-list"></div>
<div class="result" id="result" style="margin-top:20px; font-weight:bold; font-size:1.3em;">Equivalent Resistance: 0 Ω</div>

<div id="canvas-container">
  <canvas id="circuitCanvas" width="680" height="250"></canvas>
</div>

<script>
  // Circuit node tipi:
  // { type: 'series' | 'parallel', children: [node, node, ...] } veya
  // { type: 'resistor', resistance: number }

  let rootCircuit = { type: 'series', children: [] }; // Başlangıçta boş seri
  let stack = [rootCircuit]; // İç içe gruplar için yığın (stack)

  // Ekleme fonksiyonları
  function getResistanceInput() {
    const val = parseFloat(document.getElementById('resistanceInput').value);
    if (isNaN(val) || val <= 0) {
      alert('Please enter a positive resistance value.');
      return null;
    }
    return val;
  }

  // Direnç ekle, en üstteki grup içine
  function addResistor() {
    const val = getResistanceInput();
    if (val === null) return;
    const currentGroup = stack[stack.length - 1];
    currentGroup.children.push({ type: 'resistor', resistance: val });
    updateCircuit();
  }

  // Yeni seri grup başlat
  function startSeries() {
    const newGroup = { type: 'series', children: [] };
    const currentGroup = stack[stack.length - 1];
    currentGroup.children.push(newGroup);
    stack.push(newGroup);
    updateCircuit();
  }

  // Yeni paralel grup başlat
  function startParallel() {
    const newGroup = { type: 'parallel', children: [] };
    const currentGroup = stack[stack.length - 1];
    currentGroup.children.push(newGroup);
    stack.push(newGroup);
    updateCircuit();
  }

  // Mevcut grup kapat
  function endGroup() {
    if (stack.length <= 1) {
      alert("No open group to end.");
      return;
    }
    stack.pop();
    updateCircuit();
  }

  // Reset
  function resetCircuit() {
    rootCircuit = { type: 'series', children: [] };
    stack = [rootCircuit];
    updateCircuit();
  }

  // Eşdeğer direnç hesaplama (recursive)
  function calculateEquivalentResistance(node) {
    if (node.type === 'resistor') {
      return node.resistance;
    }
    if (node.type === 'series') {
      return node.children.reduce((sum, child) => sum + calculateEquivalentResistance(child), 0);
    }
    if (node.type === 'parallel') {
      let invSum = 0;
      for (const child of node.children) {
        let r = calculateEquivalentResistance(child);
        if (r === 0) return 0; // Sonsuz veya hata yok
        invSum += 1 / r;
      }
      return invSum === 0 ? 0 : 1 / invSum;
    }
    return 0;
  }

  // Metinsel gösterim (recursive)
  function renderCircuit(node, indent = '') {
    if (node.type === 'resistor') {
      return indent + `Resistor: ${node.resistance.toFixed(2)} Ω\n`;
    }
    let str = indent + (node.type === 'series' ? 'Series {\n' : 'Parallel {\n');
    node.children.forEach(child => {
      str += renderCircuit(child, indent + '  ');
    });
    str += indent + '}\n';
    return str;
  }

  // Basit çizim için (yalnızca seri ve paralel blokları yatay / dikey çizimle gösterir)
  function drawCircuit() {
    const canvas = document.getElementById('circuitCanvas');
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const startX = 20;
    const startY = canvas.height / 2;
    const resistorW = 60;
    const resistorH = 30;
    const spacing = 20;

    // Recursive çizim fonksiyonu
    function drawNode(node, x, y) {
      if (node.type === 'resistor') {
        ctx.strokeRect(x, y - resistorH / 2, resistorW, resistorH);
        ctx.fillText(`${node.resistance.toFixed(2)}Ω`, x + 5, y + 5);
        return { width: resistorW + spacing, height: resistorH };
      }
      if (node.type === 'series') {
        // Yatay yan yana
        let curX = x;
        let maxHeight = 0;
        node.children.forEach(child => {
          const size = drawNode(child, curX, y);
          curX += size.width;
          if (size.height > maxHeight) maxHeight = size.height;
        });
        // Baş ve son hatlar
        ctx.beginPath();
        ctx.moveTo(x - spacing/2, y);
        ctx.lineTo(curX - spacing, y);
        ctx.stroke();
        return { width: curX - x, height: maxHeight };
      }
      if (node.type === 'parallel') {
        // Dikey üst üste, yatay hatlar sağdan soldan bağlar
        let curY = y - (node.children.length - 1) * (resistorH + spacing) / 2;
        let maxWidth = 0;
        node.children.forEach(child => {
          const size = drawNode(child, x + spacing + resistorW, curY);
          // Sol ve sağ yatay hatlar
          ctx.beginPath();
          ctx.moveTo(x, curY);
          ctx.lineTo(x + spacing, curY);
          ctx.moveTo(x + spacing + resistorW + size.width - resistorW, curY);
          ctx.lineTo(x + spacing + resistorW + size.width, curY);
          ctx.stroke();
          curY += resistorH + spacing;
          if (size.width > maxWidth) maxWidth = size.width;
        });
        // Sol ve sağ ana dikey hatlar
        ctx.beginPath();
        ctx.moveTo(x, y - (node.children.length -1)*(resistorH+spacing)/2 - spacing/2);
        ctx.lineTo(x, y + (node.children.length -1)*(resistorH+spacing)/2 + spacing/2);
        ctx.moveTo(x + spacing + resistorW + maxWidth, y - (node.children.length -1)*(resistorH+spacing)/2 - spacing/2);
        ctx.lineTo(x + spacing + resistorW + maxWidth, y + (node.children.length -1)*(resistorH+spacing)/2 + spacing/2);
        ctx.stroke();

        return { width: spacing + resistorW + maxWidth, height: (resistorH + spacing) * node.children.length };
      }
    }

    ctx.strokeStyle = '#333';
    ctx.lineWidth = 2;
    ctx.font = '14px Arial';
    drawNode(rootCircuit, startX, startY);
  }

  // Güncelleme fonksiyonu
  function updateCircuit() {
    const eq = calculateEquivalentResistance(rootCircuit);
    document.getElementById('result').innerText = `Equivalent Resistance: ${eq.toFixed(3)} Ω`;

    document.getElementById('resistor-list').innerText = renderCircuit(rootCircuit);
    drawCircuit();
  }

  resetCircuit();
</script>
