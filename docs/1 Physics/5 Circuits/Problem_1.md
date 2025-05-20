<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Physics Lab Report</title>
    <style>
        :root {
            --primary: #2980b9;
            --secondary: #7f8c8d;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            max-width: 800px;
            margin: 20px auto;
            padding: 0 20px;
            line-height: 1.6;
        }
        h1, h2, h3 {
            color: var(--primary);
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
        }
        .simulation-box {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 5px;
            margin: 20px 0;
        }
        .controls {
            display: flex;
            gap: 10px;
            margin: 15px 0;
        }
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 4px;
            cursor: pointer;
        }
        canvas {
            border: 1px solid var(--secondary);
            margin: 10px 0;
        }
        .formula {
            background: #f0f3f4;
            padding: 10px;
            border-radius: 3px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <h1>Circuit Analysis Report</h1>

    <!-- Theory Section -->
    <h2>1. Theoretical Background</h2>
    <div class="formula">
        <strong>Series Resistance:</strong> R<sub>eq</sub> = R₁ + R₂ + ... + Rₙ
    </div>
    <div class="formula">
        <strong>Parallel Resistance:</strong> 1/R<sub>eq</sub> = 1/R₁ + 1/R₂ + ... + 1/Rₙ
    </div>

    <!-- Algorithm Section -->
    <h2>2. Reduction Algorithm</h2>
    <pre>
function reduceGraph(graph):
    while changes:
        if series nodes exist:
            merge resistors in series
        elif parallel edges exist:
            merge resistors in parallel
    return final resistance
    </pre>

    <!-- Simulation Section -->
    <div class="simulation-box">
        <h3>Interactive Circuit Simulator</h3>
        <canvas id="circuitCanvas" width="600" height="200"></canvas>
        
        <div class="controls">
            <button onclick="addComponent('series')">Add Series Resistor</button>
            <button onclick="addComponent('parallel')">Add Parallel Resistor</button>
            <button onclick="reset()" style="background: var(--secondary)">Reset</button>
        </div>
        
        <div>
            <strong>Resistance Value:</strong>
            <input type="number" id="resValue" value="10" min="1" style="width: 80px;">
            Ω
        </div>
        
        <h4 style="margin-top:15px;">Equivalent Resistance: 
            <span id="result" style="color: var(--primary);">0</span> Ω
        </h4>
    </div>

    <!-- Analysis Section -->
    <h2>3. Complexity Analysis</h2>
    <table style="width:100%; border-collapse: collapse;">
        <tr style="background: var(--primary); color: white;">
            <th style="padding: 8px;">Operation</th>
            <th>Time Complexity</th>
        </tr>
        <tr style="border-bottom: 1px solid #ddd;">
            <td style="padding: 8px;">Series Reduction</td>
            <td>O(n)</td>
        </tr>
        <tr style="border-bottom: 1px solid #ddd;">
            <td style="padding: 8px;">Parallel Reduction</td>
            <td>O(n log n)</td>
        </tr>
    </table>

<script>
// Circuit Simulation Logic
let components = [];
const canvas = document.getElementById('circuitCanvas');
const ctx = canvas.getContext('2d');

function drawComponent(x, y, type) {
    ctx.beginPath();
    ctx.moveTo(x, y);
    
    // Draw resistor symbol
    for(let i = 0; i < 4; i++) {
        ctx.lineTo(x + 20 + i*30, y + (i%2 ? -15 : 15));
    }
    
    ctx.strokeStyle = type === 'series' ? '#e74c3c' : '#3498db';
    ctx.lineWidth = 2;
    ctx.stroke();
}

function drawCircuit() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // Draw base line
    ctx.beginPath();
    ctx.moveTo(50, 100);
    ctx.lineTo(canvas.width-50, 100);
    ctx.strokeStyle = '#333';
    ctx.stroke();

    // Draw components
    let x = 100;
    components.forEach(comp => {
        drawComponent(x, 100, comp.type);
        x += 80;
    });
}

function calculateResistance() {
    let seriesSum = 0;
    let parallelSum = 0;
    
    components.forEach(comp => {
        const value = parseFloat(comp.value);
        comp.type === 'series' ? seriesSum += value : parallelSum += 1/value;
    });
    
    const total = seriesSum + (parallelSum > 0 ? 1/parallelSum : 0);
    document.getElementById('result').textContent = total.toFixed(2);
}

function addComponent(type) {
    const value = document.getElementById('resValue').value;
    components.push({type: type, value: value});
    calculateResistance();
    drawCircuit();
}

function reset() {
    components = [];
    calculateResistance();
    drawCircuit();
}

// Initial draw
drawCircuit();
</script>
</body>
</html>