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
  <meta charset="UTF-8">
  <title>2D & 3D Lorentz Simülasyonu</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
    }

    .section {
      border-bottom: 2px solid #ddd;
      padding: 20px;
    }

    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 10px;
    }

    .controls label {
      min-width: 90px;
    }

    canvas {
      border: 1px solid #ccc;
      width: 100%;
      height: 400px;
      display: block;
    }

    #container3d {
      width: 100%;
      height: 500px;
    }
  </style>
</head>
<body>

<!-- 2D Simülasyon Bölümü -->
<div class="section">
  <h2>2D Lorentz Kuvveti Simülasyonu</h2>
  <div class="controls">
    <label>Ex: <input type="number" id="Ex2D" value="0"></label>
    <label>Ey: <input type="number" id="Ey2D" value="0"></label>
    <label>Bz: <input type="number" id="Bz2D" value="1"></label>
    <label>vx: <input type="number" id="vx2D" value="5"></label>
    <label>vy: <input type="number" id="vy2D" value="0"></label>
    <label>q: <input type="number" id="q2D" value="1"></label>
    <label>m: <input type="number" id="m2D" value="1"></label>
    <button onclick="run2DSimulation()">Simulate 2D</button>
  </div>
  <canvas id="canvas2d"></canvas>
</div>

<!-- 3D Simülasyon Bölümü -->
<div class="section">
  <h2>3D Lorentz Kuvveti Simülasyonu</h2>
  <div class="controls">
    <label>Ex: <input type="number" id="Ex3D" value="0"></label>
    <label>Ey: <input type="number" id="Ey3D" value="0"></label>
    <label>Ez: <input type="number" id="Ez3D" value="0"></label>
    <label>Bx: <input type="number" id="Bx3D" value="0"></label>
    <label>By: <input type="number" id="By3D" value="0"></label>
    <label>Bz: <input type="number" id="Bz3D" value="1"></label>
    <label>vx: <input type="number" id="vx3D" value="5"></label>
    <label>vy: <input type="number" id="vy3D" value="0"></label>
    <label>vz: <input type="number" id="vz3D" value="0"></label>
    <label>q: <input type="number" id="q3D" value="1"></label>
    <label>m: <input type="number" id="m3D" value="1"></label>
    <button onclick="run3DSimulation()">Simulate 3D</button>
  </div>
  <div id="container3d"></div>
</div>

<!-- Three.js Kütüphanesi -->
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/examples/js/controls/OrbitControls.js"></script>

<script>
  // ==== 2D SIMÜLASYON ====
  const canvas2d = document.getElementById("canvas2d");
  const ctx2d = canvas2d.getContext("2d");

  function run2DSimulation() {
    const Ex = parseFloat(document.getElementById("Ex2D").value);
    const Ey = parseFloat(document.getElementById("Ey2D").value);
    const Bz = parseFloat(document.getElementById("Bz2D").value);
    const vx0 = parseFloat(document.getElementById("vx2D").value);
    const vy0 = parseFloat(document.getElementById("vy2D").value);
    const q = parseFloat(document.getElementById("q2D").value);
    const m = parseFloat(document.getElementById("m2D").value);

    canvas2d.width = canvas2d.clientWidth;
    canvas2d.height = 400;

    ctx2d.clearRect(0, 0, canvas2d.width, canvas2d.height);
    ctx2d.beginPath();

    let x = 0, y = 0;
    let vx = vx0, vy = vy0;
    const dt = 0.02, steps = 2000, scale = 5;

    ctx2d.moveTo(canvas2d.width / 2, canvas2d.height / 2);

    for (let i = 0; i < steps; i++) {
      const Fx = q * (Ex + vy * Bz);
      const Fy = q * (Ey - vx * Bz);
      const ax = Fx / m, ay = Fy / m;
      vx += ax * dt;
      vy += ay * dt;
      x += vx * dt;
      y += vy * dt;
      ctx2d.lineTo(canvas2d.width / 2 + x * scale, canvas2d.height / 2 - y * scale);
    }

    ctx2d.strokeStyle = "blue";
    ctx2d.stroke();
  }

  // ==== 3D SIMÜLASYON ====
  let scene, camera, renderer, controls, line;
  const container3d = document.getElementById("container3d");

  function init3D() {
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(75, container3d.clientWidth / container3d.clientHeight, 0.1, 1000);
    camera.position.set(50, 50, 50);

    renderer = new THREE.WebGLRenderer();
    renderer.setSize(container3d.clientWidth, container3d.clientHeight);
    container3d.appendChild(renderer.domElement);

    controls = new THREE.OrbitControls(camera, renderer.domElement);
    scene.add(new THREE.AxesHelper(20));

    animate();
  }

  function run3DSimulation() {
    if (line) scene.remove(line);

    const Ex = parseFloat(document.getElementById("Ex3D").value);
    const Ey = parseFloat(document.getElementById("Ey3D").value);
    const Ez = parseFloat(document.getElementById("Ez3D").value);
    const Bx = parseFloat(document.getElementById("Bx3D").value);
    const By = parseFloat(document.getElementById("By3D").value);
    const Bz = parseFloat(document.getElementById("Bz3D").value);
    const vx0 = parseFloat(document.getElementById("vx3D").value);
    const vy0 = parseFloat(document.getElementById("vy3D").value);
    const vz0 = parseFloat(document.getElementById("vz3D").value);
    const q = parseFloat(document.getElementById("q3D").value);
    const m = parseFloat(document.getElementById("m3D").value);

    let x = 0, y = 0, z = 0;
    let vx = vx0, vy = vy0, vz = vz0;
    const dt = 0.02, steps = 3000;
    const points = [];

    for (let i = 0; i < steps; i++) {
      const Fx = q * (Ex + vy * Bz - vz * By);
      const Fy = q * (Ey + vz * Bx - vx * Bz);
      const Fz = q * (Ez + vx * By - vy * Bx);
      const ax = Fx / m, ay = Fy / m, az = Fz / m;
      vx += ax * dt;
      vy += ay * dt;
      vz += az * dt;
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

  init3D();
</script>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>3D Test</title>
  <style>
    body { margin: 0; }
    #container3d { width: 100%; height: 100vh; }
  </style>
</head>
<body>
  <div id="container3d"></div>
  <script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.157.0/examples/js/controls/OrbitControls.js"></script>
  <script>
    const container = document.getElementById('container3d');
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 0.1, 1000);
    camera.position.set(30, 30, 30);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    scene.add(new THREE.AxesHelper(20));

    const geometry = new THREE.BoxGeometry();
    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00, wireframe: true });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);

    function animate() {
      requestAnimationFrame(animate);
      controls.update();
      renderer.render(scene, camera);
    }
    animate();
  </script>
</body>
</html>


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
