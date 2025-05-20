# Electromagnetism: Lorentz Force Simulation

## Problem 1: Simulating the Effects of the Lorentz Force

### Motivation

The **Lorentz force** describes the combined effect of electric and magnetic fields on a moving charged particle. It is a cornerstone of electromagnetism and plays a crucial role in:

- **Plasma physics**
- **Particle accelerators**
- **Mass spectrometry**
- **Astrophysical plasmas**
- **Fusion devices (e.g., Tokamaks)**

The Lorentz force is defined by the following vector equation:

\[
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
\]

Where:
- \( \vec{F} \) is the force experienced by the particle (in newtons)
- \( q \) is the charge of the particle (in coulombs)
- \( \vec{E} \) is the electric field (in volts per meter)
- \( \vec{v} \) is the velocity of the particle (in meters per second)
- \( \vec{B} \) is the magnetic field (in teslas)

Understanding the behavior of charged particles in various field configurations allows us to model and design devices used in modern physics and engineering.

---

### 1. Exploration of Applications

#### Real-World Systems Where Lorentz Force is Crucial:

- **Particle Accelerators:** Steering and focusing charged particles using magnetic and electric fields.
- **Mass Spectrometers:** Separation of ions based on mass-to-charge ratio using magnetic deflection.
- **Plasma Confinement Devices:** Magnetic fields are used to confine hot plasma in fusion reactors like Tokamaks.
- **Cathode Ray Tubes (CRTs):** Electron beams are deflected using magnetic and electric fields.

#### Role of Fields:

- **Electric Fields (\( \vec{E} \))**: Accelerate particles in the direction of the field.
- **Magnetic Fields (\( \vec{B} \))**: Cause circular or helical motion due to perpendicular force to velocity.

---

### 2. Simulating Particle Motion

We simulate the motion of a charged particle under various field configurations using Newton’s Second Law:

\[
m\frac{d\vec{v}}{dt} = q(\vec{E} + \vec{v} \times \vec{B})
\]

Where:
- \( m \) is the mass of the particle
- \( \frac{d\vec{v}}{dt} \) is the particle’s acceleration

#### Scenarios:

- **Uniform Magnetic Field Only:**
  - Particle undergoes circular or helical motion depending on velocity components.
  - Radius of motion (Larmor radius) is:

    \[
    r = \frac{mv_\perp}{|q|B}
    \]

- **Combined Uniform Electric and Magnetic Fields:**
  - Results in **drift motion** or **helical trajectories**.
  - If \( \vec{E} \perp \vec{B} \), particle drifts at:

    \[
    \vec{v}_d = \frac{\vec{E} \times \vec{B}}{B^2}
    \]

- **Crossed Fields (\( \vec{E} \perp \vec{B} \)):**
  - The particle can exhibit complex motion with both circular and translational components.

---

### 3. Parameter Exploration

The simulation will support variable parameters:

- **Field Strengths:**
  - Electric field magnitude and direction \( \vec{E} \)
  - Magnetic field magnitude and direction \( \vec{B} \)

- **Particle Properties:**
  - Initial velocity \( \vec{v}_0 \)
  - Charge \( q \)
  - Mass \( m \)

By changing these, we observe different behaviors:

- Increasing \( B \) → smaller Larmor radius
- Increasing \( E \) → increased drift velocity
- Heavier mass \( m \) → slower response to field changes

---

### 4. Visualization

We aim to produce labeled and annotated plots showing:

- **2D and 3D Trajectories**:
  - Top-down views for circular motion
  - Side views for helical motion
- **Key Physical Quantities**:
  - Larmor Radius (\( r \))
  - Drift Velocity (\( v_d \))

Visualizations provide intuition about the dynamics under different configurations and help relate abstract concepts to practical applications.

---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Lorentz Force Simulator</title>
  <style>
    canvas { border: 1px solid #000; background: #f8f8f8; }
    input { width: 60px; margin-right: 10px; }
        body { margin: 0; overflow: hidden; font-family: sans-serif; }
    #controls {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(255,255,255,0.9);
      padding: 10px;
      border-radius: 8px;
      z-index: 10;
    }
    label { display: block; margin: 5px 0; }
    input { width: 60px; }
    canvas { display: block; }
    
  </style>
</head>
<body>
  <h2>Lorentz Force Simulator (2D)</h2>

  <div>
    <label>Electric Field Ex (V/m): <input type="number" id="Ex" value="0" /></label>
    <label>Magnetic Field Bz (T): <input type="number" id="Bz" value="1" /></label>
    <label>Initial Velocity vx (m/s): <input type="number" id="vx" value="10" /></label>
    <label>vy (m/s): <input type="number" id="vy" value="0" /></label>
    <label>Charge q (C): <input type="number" id="q" value="1" /></label>
    <label>Mass m (kg): <input type="number" id="m" value="1" /></label>
    <button onclick="startSimulation()">Start</button>
  </div>

  <canvas id="canvas" width="600" height="600"></canvas>


<div id="controls">
  <label>Ex: <input type="number" id="Ex" value="0" /></label>
  <label>Ey: <input type="number" id="Ey" value="0" /></label>
  <label>Ez: <input type="number" id="Ez" value="0" /></label>
  <label>Bx: <input type="number" id="Bx" value="0" /></label>
  <label>By: <input type="number" id="By" value="0" /></label>
  <label>Bz: <input type="number" id="Bz" value="1" /></label>
  <label>vx: <input type="number" id="vx" value="5" /></label>
  <label>vy: <input type="number" id="vy" value="0" /></label>
  <label>vz: <input type="number" id="vz" value="0" /></label>
  <label>Charge (q): <input type="number" id="q" value="1" /></label>
  <label>Mass (m): <input type="number" id="m" value="1" /></label>
  <button onclick="startSimulation()">Start</button>
</div>



<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/examples/js/controls/OrbitControls.js"></script>
  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const width = canvas.width;
    const height = canvas.height;

    function startSimulation() {
      // Read parameters
      const Ex = parseFloat(document.getElementById('Ex').value);
      const Bz = parseFloat(document.getElementById('Bz').value);
      const vx0 = parseFloat(document.getElementById('vx').value);
      const vy0 = parseFloat(document.getElementById('vy').value);
      const q = parseFloat(document.getElementById('q').value);
      const m = parseFloat(document.getElementById('m').value);

      // Initial state
      let x = width / 2, y = height / 2;
      let vx = vx0, vy = vy0;
      const dt = 0.1;

      // Clear canvas
      ctx.clearRect(0, 0, width, height);
      ctx.beginPath();
      ctx.moveTo(x, y);

      let steps = 2000;
      function step() {
        for (let i = 0; i < 10; i++) {
          const Fx = q * (Ex + vy * Bz);
          const Fy = q * (-vx * Bz);

          const ax = Fx / m;
          const ay = Fy / m;

          vx += ax * dt;
          vy += ay * dt;
          x += vx * dt;
          y += vy * dt;

          ctx.lineTo(x, y);
        }
        ctx.stroke();

        steps -= 10;
        if (steps > 0) requestAnimationFrame(step);
      }

      step();
    }

      let scene, camera, renderer, controls, line;
  let points = [];

  function initScene() {
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
    camera.position.set(50, 50, 50);

    renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    controls = new THREE.OrbitControls(camera, renderer.domElement);
    scene.add(new THREE.AxesHelper(20));
  }

  function startSimulation() {
    if (line) scene.remove(line);
    points = [];

    // Read input
    const Ex = parseFloat(document.getElementById("Ex").value);
    const Ey = parseFloat(document.getElementById("Ey").value);
    const Ez = parseFloat(document.getElementById("Ez").value);
    const Bx = parseFloat(document.getElementById("Bx").value);
    const By = parseFloat(document.getElementById("By").value);
    const Bz = parseFloat(document.getElementById("Bz").value);
    let vx = parseFloat(document.getElementById("vx").value);
    let vy = parseFloat(document.getElementById("vy").value);
    let vz = parseFloat(document.getElementById("vz").value);
    const q = parseFloat(document.getElementById("q").value);
    const m = parseFloat(document.getElementById("m").value);

    let x = 0, y = 0, z = 0;
    const dt = 0.02;
    const steps = 3000;

    for (let i = 0; i < steps; i++) {
      // Lorentz force F = q(E + v × B)
      const Fx = q * (Ex + vy * Bz - vz * By);
      const Fy = q * (Ey + vz * Bx - vx * Bz);
      const Fz = q * (Ez + vx * By - vy * Bx);

      // acceleration
      const ax = Fx / m;
      const ay = Fy / m;
      const az = Fz / m;

      // update velocity
      vx += ax * dt;
      vy += ay * dt;
      vz += az * dt;

      // update position
      x += vx * dt;
      y += vy * dt;
      z += vz * dt;

      points.push(new THREE.Vector3(x, y, z));
    }

    const geometry = new THREE.BufferGeometry().setFromPoints(points);
    const material = new THREE.LineBasicMaterial({ color: 0xff0000 });
    line = new THREE.Line(geometry, material);
    scene.add(line);
  }

  function animate() {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  }

  initScene();
  animate();
  </script>




</body>
</html>



---

### Suggestions for Extensions

- Simulate **non-uniform magnetic fields** to model magnetic mirrors or advanced trap designs.
- Incorporate **relativistic effects** for particles approaching light speed.
- Add **collision effects** to simulate plasma interactions.

---

### References

- Griffiths, D. J. *Introduction to Electrodynamics*
- Jackson, J. D. *Classical Electrodynamics*
- Numerical Recipes in Python: Runge-Kutta Methods
