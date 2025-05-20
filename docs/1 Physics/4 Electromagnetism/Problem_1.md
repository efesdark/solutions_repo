# Simulating the Effects of the Lorentz Force

## 1. Introduction and Applications

The Lorentz force, expressed as **F = q(E + v × B)**, governs the motion of charged particles in electric and magnetic fields. It is fundamental in fields such as plasma physics, particle accelerators, and astrophysics.

Key systems involving Lorentz force:
- Particle accelerators (e.g., cyclotrons)
- Mass spectrometers
- Magnetic confinement in plasma physics

Electric fields (E) accelerate charged particles, while magnetic fields (B) change their direction of motion, leading to complex trajectories.

## 2. Simulation of Particle Motion

We implement a numerical simulation of a charged particle's trajectory under various field conditions:

- Uniform magnetic field
- Combined uniform electric and magnetic fields
- Crossed electric and magnetic fields

Parameters such as charge (q), mass (m), initial velocity (v), and field strengths (E, B) can be adjusted.

## 3. Numerical Method

We use the Euler method for integrating the equations of motion:

\[
\mathbf{F} = q(\mathbf{E} + \mathbf{v} \times \mathbf{B}) = m \frac{d\mathbf{v}}{dt}
\]

At each time step:

\[
\mathbf{a} = \frac{\mathbf{F}}{m}
\]
\[
\mathbf{v}_{t+1} = \mathbf{v}_t + \mathbf{a} \Delta t
\]
\[
\mathbf{r}_{t+1} = \mathbf{r}_t + \mathbf{v}_{t+1} \Delta t
\]

## 4. Interactive Simulation

Below is an interactive JavaScript simulation embedded directly in the page. You can modify parameters and visualize the particle's trajectory in 2D.

<div>
  <label>Charge (q): <input id="q" type="number" value="1" step="0.1"></label>
  <label>Mass (m): <input id="m" type="number" value="1" step="0.1"></label>
  <label>Electric Field Ex: <input id="Ex" type="number" value="0" step="0.1"></label>
  <label>Electric Field Ey: <input id="Ey" type="number" value="0" step="0.1"></label>
  <label>Magnetic Field Bz: <input id="Bz" type="number" value="1" step="0.1"></label><br>
  <label>Initial Velocity Vx: <input id="Vx" type="number" value="1" step="0.1"></label>
  <label>Initial Velocity Vy: <input id="Vy" type="number" value="0" step="0.1"></label>
  <button onclick="runSimulation()">Run Simulation</button>
</div>

<canvas id="lorentzCanvas" width="600" height="400" style="border:1px solid #000;"></canvas>

<script>
function runSimulation() {
  const q = parseFloat(document.getElementById("q").value);
  const m = parseFloat(document.getElementById("m").value);
  const Ex = parseFloat(document.getElementById("Ex").value);
  const Ey = parseFloat(document.getElementById("Ey").value);
  const Bz = parseFloat(document.getElementById("Bz").value);
  let Vx = parseFloat(document.getElementById("Vx").value);
  let Vy = parseFloat(document.getElementById("Vy").value);

  const canvas = document.getElementById("lorentzCanvas");
  const ctx = canvas.getContext("2d");
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Initial position in middle
  let x = canvas.width / 2;
  let y = canvas.height / 2;

  const dt = 0.1; 
  ctx.beginPath();
  ctx.moveTo(x, y);

  for(let i=0; i<1000; i++) {
    // Lorentz Force components
    const Fx = q * (Ex + Vy * Bz);
    const Fy = q * (Ey - Vx * Bz);

    // Acceleration
    const ax = Fx / m;
    const ay = Fy / m;

    // Update velocity
    Vx += ax * dt;
    Vy += ay * dt;

    // Update position
    x += Vx;
    y += Vy;

    ctx.lineTo(x, y);
  }

  ctx.strokeStyle = "blue";
  ctx.lineWidth = 2;
  ctx.stroke();
}
</script>

---

## 5. Discussion

The results demonstrate classical behaviors such as:

- Circular motion in uniform magnetic fields (Larmor orbits)
- Helical paths with combined E and B fields
- Drift motion with crossed fields

These phenomena underpin technologies like cyclotrons and magnetic traps used in plasma confinement.

---

## 6. Extensions

Possible extensions include:

- 3D trajectory visualization
- More accurate numerical methods (Runge-Kutta)
- Non-uniform or time-dependent fields
- Multiple particle simulations



