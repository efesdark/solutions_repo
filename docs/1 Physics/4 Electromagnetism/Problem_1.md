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
<!-- ===================== 2D SIMULATION ===================== -->
<h2>2D Lorentz Force Simulation</h2>
<div>
  <label>Initial Velocity X: <input type="number" id="vx2d" value="1"></label>
  <label>Y: <input type="number" id="vy2d" value="1"></label>
  <button onclick="simulate2D()">Simulate 2D</button>
</div>
<canvas id="canvas2d" width="500" height="400" style="border:1px solid #000;"></canvas>

<script>
function simulate2D() {
  const canvas = document.getElementById("canvas2d");
  const ctx = canvas.getContext("2d");
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  const q = 1, m = 1, Bz = 1;
  let vx = parseFloat(document.getElementById("vx2d").value);
  let vy = parseFloat(document.getElementById("vy2d").value);
  let x = canvas.width / 2;
  let y = canvas.height / 2;
  const dt = 0.1;

  ctx.beginPath();
  ctx.moveTo(x, y);

  for (let i = 0; i < 1000; i++) {
    const Fx = q * vy * Bz;
    const Fy = -q * vx * Bz;
    vx += (Fx / m) * dt;
    vy += (Fy / m) * dt;
    x += vx;
    y += vy;
    ctx.lineTo(x, y);
  }

  ctx.strokeStyle = "blue";
  ctx.stroke();
}
</script>

<hr>

<!-- ===================== 3D SIMULATION ===================== -->
<h2>3D Lorentz Force Simulation</h2>
<div>
  <label>Initial Velocity X: <input type="number" id="vx3d" value="1"></label>
  <label>Y: <input type="number" id="vy3d" value="1"></label>
  <label>Z: <input type="number" id="vz3d" value="1"></label>
  <button onclick="init3D()">Simulate 3D</button>
</div>
<div id="canvas3d" style="width: 100%; height: 400px; border: 1px solid #000;"></div>

<!-- Load Three.js and OrbitControls -->
<script src="https://unpkg.com/three@0.157.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.152.2/examples/js/controls/OrbitControls.js"></script>


<script>
function init3D() {
  const container = document.getElementById("canvas3d");
  container.innerHTML = ""; // Clear previous render

  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(container.clientWidth, container.clientHeight);
  container.appendChild(renderer.domElement);

  const controls = new THREE.OrbitControls(camera, renderer.domElement);
  camera.position.set(10, 10, 10);
  controls.update();

  const q = 1, m = 1;
  const B = new THREE.Vector3(0, 0, 1);
  let v = new THREE.Vector3(
    parseFloat(document.getElementById("vx3d").value),
    parseFloat(document.getElementById("vy3d").value),
    parseFloat(document.getElementById("vz3d").value)
  );
  let pos = new THREE.Vector3(0, 0, 0);
  const dt = 0.05;

  const geometry = new THREE.BufferGeometry();
  const positions = [];

  for (let i = 0; i < 1000; i++) {
    const F = new THREE.Vector3().crossVectors(v, B).multiplyScalar(q / m);
    v.add(F.clone().multiplyScalar(dt));
    pos.add(v.clone().multiplyScalar(dt));
    positions.push(pos.x, pos.y, pos.z);
  }

  geometry.setAttribute('position', new THREE.Float32BufferAttribute(positions, 3));
  const material = new THREE.LineBasicMaterial({ color: 0xff0000 });
  const line = new THREE.Line(geometry, material);
  scene.add(line);

  scene.add(new THREE.AxesHelper(5));

  function animate() {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  }

  animate();
}
</script>




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
