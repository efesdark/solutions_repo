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
  <title>2D & 3D Lorentz Force Simulator</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
    }
    #controls {
      padding: 10px;
      background: #f0f0f0;
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    #controls label {
      margin-right: 10px;
    }
    #simulations {
      display: flex;
      flex-direction: row;
      height: calc(100vh - 100px);
    }
    #canvas2d {
      flex: 1;
      border: 1px solid #ccc;
    }
    #container3d {
      flex: 1;
      position: relative;
    }
    canvas {
      width: 100%;
      height: 100%;
      display: block;
    }
  </style>
</head>
<body>

<div id="controls">
  <label>Ex: <input type="number" id="Ex" value="0"></label>
  <label>Ey: <input type="number" id="Ey" value="0"></label>
  <label>Ez: <input type="number" id="Ez" value="0"></label>
  <label>Bx: <input type="number" id="Bx" value="0"></label>
  <label>By: <input type="number" id="By" value="0"></label>
  <label>Bz: <input type="number" id="Bz" value="1"></label>
  <label>vx: <input type="number" id="vx" value="5"></label>
  <label>vy: <input type="number" id="vy" value="0"></label>
  <label>vz: <input type="number" id="vz" value="0"></label>
  <label>q: <input type="number" id="q" value="1"></label>
  <label>m: <input type="number" id="m" value="1"></label>
  <button onclick="startSimulations()">Simulate</button>
</div>

<div id="simulations">
  <canvas id="canvas2d"></canvas>
  <div id="container3d"></div>
</div>

<!-- Three.js Kütüphanesi -->
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/examples/js/controls/OrbitControls.js"></script>

<script>
  // Ortak parametre okuma fonksiyonu
  function getParams() {
    return {
      Ex: parseFloat(document.getElementById("Ex").value),
      Ey: parseFloat(document.getElementById("Ey").value),
      Ez: parseFloat(document.getElementById("Ez").value),
      Bx: parseFloat(document.getElementById("Bx").value),
      By: parseFloat(document.getElementById("By").value),
      Bz: parseFloat(document.getElementById("Bz").value),
      vx: parseFloat(document.getElementById("vx").value),
      vy: parseFloat(document.getElementById("vy").value),
      vz: parseFloat(document.getElementById("vz").value),
      q: parseFloat(document.getElementById("q").value),
      m: parseFloat(document.getElementById("m").value)
    };
  }

  // ===== 2D Simülasyon =====
  const canvas2d = document.getElementById("canvas2d");
  const ctx2d = canvas2d.getContext("2d");

  function run2DSimulation(params) {
    const { Ex, Ey, Bz, vx, vy, q, m } = params;
    canvas2d.width = canvas2d.clientWidth;
    canvas2d.height = canvas2d.clientHeight;
    ctx2d.clearRect(0, 0, canvas2d.width, canvas2d.height);

    let x = 0, y = 0;
    let vx_ = vx, vy_ = vy;
    const dt = 0.02;
    const steps = 2000;
    const scale = 5;

    ctx2d.beginPath();
    ctx2d.moveTo(canvas2d.width / 2, canvas2d.height / 2);

    for (let i = 0; i < steps; i++) {
      const Fx = q * (Ex + vy_ * Bz);
      const Fy = q * (Ey - vx_ * Bz);
      const ax = Fx / m;
      const ay = Fy / m;

      vx_ += ax * dt;
      vy_ += ay * dt;

      x += vx_ * dt;
      y += vy_ * dt;

      ctx2d.lineTo(canvas2d.width / 2 + x * scale, canvas2d.height / 2 - y * scale);
    }

    ctx2d.strokeStyle = "blue";
    ctx2d.stroke();
  }

  // ===== 3D Simülasyon =====
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

  function run3DSimulation(params) {
    if (line) scene.remove(line);
    const { Ex, Ey, Ez, Bx, By, Bz, vx, vy, vz, q, m } = params;

    let x = 0, y = 0, z = 0;
    let vx_ = vx, vy_ = vy, vz_ = vz;
    const dt = 0.02;
    const steps = 3000;
    const points = [];

    for (let i = 0; i < steps; i++) {
      const Fx = q * (Ex + vy_ * Bz - vz_ * By);
      const Fy = q * (Ey + vz_ * Bx - vx_ * Bz);
      const Fz = q * (Ez + vx_ * By - vy_ * Bx);

      const ax = Fx / m;
      const ay = Fy / m;
      const az = Fz / m;

      vx_ += ax * dt;
      vy_ += ay * dt;
      vz_ += az * dt;

      x += vx_ * dt;
      y += vy_ * dt;
      z += vz_ * dt;

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

  // === Başlatıcı ===
  function startSimulations() {
    const params = getParams();
    run2DSimulation(params);
    run3DSimulation(params);
  }

  init3D();
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
