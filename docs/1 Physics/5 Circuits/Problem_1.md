# Circuits

## Problem 1  
### Equivalent Resistance Using Graph Theory

---

## Motivation

Calculating equivalent resistance is fundamental in circuit analysis. Complex circuits often combine series and parallel resistors in nested configurations. Representing circuits as hierarchical structures allows precise computation of equivalent resistance using recursive formulas.

---

## Formulas

- **Series**: \( R_{eq} = R_1 + R_2 + \cdots + R_n \)  
- **Parallel**: \( \frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n} \)

---

## Recursive Calculation

Each group is either:

- A single resistor (leaf), or
- A group of resistors connected in series or parallel.

Equivalent resistance is computed recursively.

---

## Interactive Circuit Simulator (Nested Series & Parallel)

You can:

- Add resistors in **series** or **parallel** to the current selected group,
- Navigate into groups to add nested connections,
- Edit resistor values,
- See the circuit visually on canvas,
- See equivalent resistance updated live.

---

<style>
  body { font-family: Arial, sans-serif; max-width: 720px; margin: auto; padding: 20px;}
  input[type=number] { width: 80px; margin-right: 10px; }
  button { margin: 5px 8px 5px 0; padding: 6px 12px; cursor: pointer; }
  #breadcrumbs { margin-bottom: 10px; }
  #resistor-list { margin-top: 15px; white-space: pre-wrap; font-family: monospace; }
  #canvas-container { text-align: center; margin-top: 20px; }
  #circuitCanvas { border: 1px solid #ccc; background: #f9f9f9; }
</style>

<div>
  <label>Resistance (Ω): <input type="number" id="resistanceInput" min="0.01" step="0.01" value="10"></label><br>
  <button onclick="addResistor('series')">Add Resistor in Series</button>
  <button onclick="addResistor('parallel')">Add Resistor in Parallel</button>
  <button onclick="goUp()">Go Up One Level</button>
  <button onclick="resetCircuit()">Reset Circuit</button>
</div>

<div id="breadcrumbs"></div>

<div id="resistor-list"></div>

<div class="result" id="result" style="margin-top:20px; font-weight:bold; font-size:1.3em;">Equivalent Resistance: 0 Ω</div>

<div id="canvas-container">
  <canvas id="circuitCanvas" width="700" height="220"></canvas>
</div>

<script>
  // Data structure: Recursive groups and resistors
  // Group: { type: 'series'|'parallel', children: [...] }
  // Leaf resistor: { type: 'resistor', resistance: number }

  let circuit = {
    type: 'series',
    children: []
  };

  // Path in tree to current group for editing/navigating
  let currentPath = [];

  // Helpers
  function getCurrentGroup() {
    let node = circuit;
    for (let idx of currentPath) {
      node = node.children[idx];
    }
    return node;
  }

  // Add resistor as a leaf in current group
  function addResistor(connectionType) {
    const val = getResistanceInput();
    if (val === null) return;

    let current = getCurrentGroup();

    // If current group type differs from requested, add new subgroup
    if (current.type !== connectionType && current.children.length > 0) {
      // Wrap existing children in new subgroup and add new resistor in that subgroup
      let oldChildren = current.children;
      current.children = [{
        type: current.type,
        children: oldChildren
      }];
      current.type = connectionType;
    }

    current.children.push({ type: 'resistor', resistance: val });
    updateUI();
  }

  // Navigate up one level in tree
  function goUp() {
    if (currentPath.length === 0) return; // Already at root
    currentPath.pop();
    updateUI();
  }

  // Reset entire circuit
  function resetCircuit() {
    circuit = { type: 'series', children: [] };
    currentPath = [];
    updateUI();
  }

  // Validate and get input resistance
  function getResistanceInput() {
    const input = document.getElementById('resistanceInput');
    const val = parseFloat(input.value);
    if (isNaN(val) || val <= 0) {
      alert('Please enter a positive resistance value.');
      return null;
    }
    return val;
  }

  // Recursive equivalent resistance calculation
  function calcResistance(node) {
    if (node.type === 'resistor') {
      return node.resistance;
    }
    if (node.type === 'series') {
      return node.children.reduce((sum, c) => sum + calcResistance(c), 0);
    }
    if (node.type === 'parallel') {
      let invSum = 0;
      for (const c of node.children) {
        let r = calcResistance(c);
        if (r === 0) return 0;
        invSum += 1/r;
      }
      return invSum === 0 ? 0 : 1/invSum;
    }
    return 0;
  }

  // Render circuit textually with navigation
  function renderText(node, indent = '', indexPath = []) {
    if (node.type === 'resistor') {
      return `${indent}- Resistor: ${node.resistance.toFixed(3)} Ω\n`;
    }

    let s = `${indent}- ${node.type.toUpperCase()} group:\n`;
    node.children.forEach((child, i) => {
      s += renderText(child, indent + '  ', indexPath.concat(i));
    });
    return s;
  }

  // Update breadcrumbs UI
  function updateBreadcrumbs() {
    let crumbs = ['Root'];
    let node = circuit;
    for (let i = 0; i < currentPath.length; i++) {
      const idx = currentPath[i];
      node = node.children[idx];
      if (node.type === 'resistor') {
        crumbs.push(`R: ${node.resistance.toFixed(1)} Ω`);
      } else {
        crumbs.push(node.type.toUpperCase());
      }
    }
    document.getElementById('breadcrumbs').innerText = 'Path: ' + crumbs.join(' > ');
  }

  // Drawing helpers

  const canvas = document.getElementById('circuitCanvas');
  const ctx = canvas.getContext('2d');

  const resistorWidth = 60;
  const resistorHeight = 30;
  const spacingX = 25;
  const spacingY = 20;

  // Draw resistor symbol with label at (x,y)
  function drawResistor(x, y, label, resistance) {
    ctx.strokeStyle = '#333';
    ctx.fillStyle = '#000';
    ctx.lineWidth = 2;

    ctx.strokeRect(x, y - resistorHeight/2, resistorWidth, resistorHeight);

    ctx.font = '12px Arial';
    ctx.fillText(label, x + 5, y - 8);
    ctx.fillText(resistance.toFixed(1) + ' Ω', x + 5, y + 15);
  }

  // Recursive drawing function
  // Returns bounding box: {width, height}
  function drawNode(node, x, y) {
    if (node.type === 'resistor') {
      drawResistor(x, y, 'R', node.resistance);
      return { width: resistorWidth + spacingX, height: resistorHeight + spacingY };
    }

    if (node.type === 'series') {
      // Draw children horizontally in series
      let curX = x;
      let maxHeight = 0;
      node.children.forEach((child, i) => {
        // Draw wire to next resistor/group
        if (i > 0) {
          ctx.beginPath();
          ctx.moveTo(curX - spacingX/2, y);
          ctx.lineTo(curX, y);
          ctx.stroke();
        }

        let size = drawNode(child, curX, y);
        curX += size.width;
        if (size.height > maxHeight) maxHeight = size.height;
      });

      // Draw wires at start and end
      ctx.beginPath();
      ctx.moveTo(x - spacingX, y);
      ctx.lineTo(x, y);
      ctx.moveTo(curX, y);
      ctx.lineTo(curX + spacingX, y);
      ctx.stroke();

      return { width: (curX - x) + spacingX * 2, height: maxHeight };
    }

    if (node.type === 'parallel') {
      // Draw children vertically in parallel
      let maxWidth = 0;
      let totalHeight = 0;
      const gap = resistorHeight + spacingY;

      node.children.forEach((child, i) => {
        let posY = y + i * gap;
        let size = drawNode(child, x + spacingX, posY);
        if (size.width > maxWidth) maxWidth = size.width;
        totalHeight += size.height;
      });

      // Draw vertical wires on left and right
      let topY = y - spacingY/2;
      let bottomY = y + (node.children.length - 1) * (resistorHeight + spacingY) + resistorHeight/2 + spacingY/2;

  ctx.beginPath();
  // Left vertical wire
  ctx.moveTo(x, topY);
  ctx.lineTo(x, bottomY);

  // Right vertical wire
  ctx.moveTo(x + maxWidth + spacingX*1.5, topY);
  ctx.lineTo(x + maxWidth + spacingX*1.5, bottomY);

  // Connect horizontal wires for each child
  node.children.forEach((child, i) => {
    let posY = y + i * gap + resistorHeight/2;
    ctx.moveTo(x, posY);
    ctx.lineTo(x + spacingX, posY);
    ctx.moveTo(x + maxWidth + spacingX * 1.5, posY);
    ctx.lineTo(x + maxWidth + spacingX, posY);
  });

  ctx.stroke();

  return { width: maxWidth + spacingX * 2.5, height: bottomY - topY };
}
// Clear and redraw canvas
function drawCircuit() {
ctx.clearRect(0, 0, canvas.width, canvas.height);
drawNode(circuit, 100, 100);
}

// Update UI elements
function updateUI() {
updateBreadcrumbs();
const res = calcResistance(circuit);
document.getElementById('result').innerText = Equivalent Resistance: ${res.toFixed(3)} Ω;
document.getElementById('resistor-list').innerText = renderText(circuit);
drawCircuit();
}

// Initial UI update
resetCircuit();

</script> ```