<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Circuit Analysis Report: Equivalent Resistance with Graph Theory</title>
    <style>
        body {font-family: Arial, sans-serif; max-width: 800px; margin: auto; padding: 20px}
        .theory {background: #f8f9fa; padding: 15px; border-radius: 5px}
        .simulation {margin: 30px 0; border: 1px solid #ddd; padding: 20px}
        canvas {border: 1px solid #999}
        input[type="number"] {width: 100px; margin: 5px}
    </style>
</head>
<body>
    <h1>Equivalent Resistance Using Graph Theory</h1>
    
    <!-- Theory Section -->
    <div class="theory">
        <h2>1. Theoretical Background</h2>
        <h3>Key Formulas</h3>
        <ul>
            <li>Series Resistors: R<sub>eq</sub> = R₁ + R₂ + ... + Rₙ</li>
            <li>Parallel Resistors: 1/R<sub>eq</sub> = 1/R₁ + 1/R₂ + ... + 1/Rₙ</li>
        </ul>

        <h3>Graph Theory Approach</h3>
        <p>Representation principles:</p>
        <pre>
        Graph G = (V, E)
        V: Nodes (circuit junctions)
        E: Edges (resistors with weights)
        </pre>

        <h3>Reduction Algorithm</h3>
        <pre>
        function equivalent_resistance(graph):
            while graph has reducible components:
                if series_node exists:
                    merge R = R1 + R2
                elif parallel_edges exist:
                    merge 1/R = 1/R1 + 1/R2
            return final resistance
        </pre>
    </div>

    <!-- Interactive Simulation -->
    <div class="simulation">
        <h2>2. Circuit Simulator</h2>
        <canvas id="circuitCanvas" width="600" height="300"></canvas>
        
        <div>
            <h3>Resistor Controls</h3>
            <button onclick="addResistor('series')">Add Series Resistor</button>
            <button onclick="addResistor('parallel')">Add Parallel Resistor</button>
            <button onclick="resetCircuit()">Reset</button><br>
            Resistance Value: <input type="number" id="resValue" value="10" min="1" step="1">
        </div>

        <div id="result" style="margin-top:15px; font-weight:bold"></div>
    </div>

    <!-- Algorithm Explanation -->
    <div class="theory">
        <h2>3. Algorithm Implementation</h2>
        <h3>Pseudocode</h3>
        <pre>
        function reduceGraph(graph):
            while changes occur:
                // Series reduction
                for node in nodes:
                    if degree(node) == 2:
                        R_total = sum(adjacent edges)
                        replace with single edge R_total
                        
                // Parallel reduction
                for edge_pair in edges:
                    if same start/end nodes:
                        R_total = 1/(sum(1/R))
                        replace with single edge R_total
        </pre>

        <h3>Complexity Analysis</h3>
        <table border="1">
            <tr><th>Operation</th><th>Time Complexity</th></tr>
            <tr><td>Series Detection</td><td>O(V)</td></tr>
            <tr><td>Parallel Detection</td><td>O(E)</td></tr>
            <tr><td>Total</td><td>O(V+E) per iteration</td></tr>
        </table>
    </div>

<script>
// Circuit Simulation Logic
let resistors = [];
let totalResistance = 0;

function calculateEquivalent() {
    let seriesSum = 0;
    let parallelSum = 0;
    
    resistors.forEach(r => {
        if(r.type === 'series') seriesSum += r.value;
        else parallelSum += 1/r.value;
    });
    
    totalResistance = seriesSum + (parallelSum > 0 ? 1/parallelSum : 0);
    document.getElementById('result').innerHTML = 
        `Equivalent Resistance: ${totalResistance.toFixed(2)} Ω`;
    drawCircuit();
}

function addResistor(type) {
    const value = parseFloat(document.getElementById('resValue').value);
    resistors.push({type: type, value: value});
    calculateEquivalent();
}

function resetCircuit() {
    resistors = [];
    calculateEquivalent();
}

// Canvas Drawing
function drawCircuit() {
    const canvas = document.getElementById('circuitCanvas');
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Draw circuit base
    ctx.beginPath();
    ctx.moveTo(50, 150);
    ctx.lineTo(550, 150);
    ctx.strokeStyle = '#333';
    ctx.stroke();

    // Draw resistors
    let x = 100;
    resistors.forEach(r => {
        drawResistor(ctx, x, 150, r.type);
        x += 100;
    });
}

function drawResistor(ctx, x, y, type) {
    ctx.beginPath();
    ctx.moveTo(x-30, y);
    ctx.lineTo(x+30, y);
    ctx.strokeStyle = type === 'series' ? '#d32f2f' : '#1976d2';
    ctx.lineWidth = 2;
    ctx.stroke();
}
</script>

</body>
</html>