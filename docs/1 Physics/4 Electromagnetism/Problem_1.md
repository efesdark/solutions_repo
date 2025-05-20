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
<html>
<head>
    <title>Lorentz Force Simulator</title>
    <style>
        .container { display: flex; }
        .controls { padding: 20px; }
        input[type="range"] { width: 200px; }
        canvas { border: 1px solid #000; }
    </style>
</head>
<body>
    <div class="container">
        <div class="controls">
            <h3>Simulation Parameters</h3>
            <label>Charge (q): <input type="range" id="charge" min="-5" max="5" step="0.1" value="1"></label>
            <span id="chargeValue">1</span><br>
            
            <label>E-Field (E): <input type="range" id="E" min="0" max="10" step="0.1" value="0"></label>
            <span id="EValue">0</span><br>
            
            <label>B-Field (B): <input type="range" id="B" min="0" max="5" step="0.1" value="1"></label>
            <span id="BValue">1</span><br>
            
            <label>Mass (m): <input type="range" id="mass" min="1" max="10" step="0.1" value="1"></label>
            <span id="massValue">1</span><br>
            
            <label>Initial Velocity (v₀): <input type="range" id="v0" min="0" max="10" step="0.1" value="2"></label>
            <span id="v0Value">2</span><br>
            
            <button onclick="resetParticle()">Reset Particle</button>
            <button onclick="toggleAnimation()">Start/Stop</button>
        </div>

        <div>
            <h4>2D Visualization</h4>
            <canvas id="canvas2D" width="600" height="400"></canvas>
            <h4>3D Visualization</h4>
            <div id="canvas3D" style="width: 600px; height: 400px;"></div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // 2D Canvas Setup
        const canvas2D = document.getElementById('canvas2D');
        const ctx = canvas2D.getContext('2d');
        
        // 3D Three.js Setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, 600/400, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(600, 400);
        document.getElementById('canvas3D').appendChild(renderer.domElement);
        
        // Particle properties
        let particle = {
            position: new THREE.Vector3(0, 0, 0),
            velocity: new THREE.Vector3(2, 0, 0),
            acceleration: new THREE.Vector3(),
            trail: []
        };

        // Create 3D particle
        const geometry = new THREE.SphereGeometry(0.2);
        const material = new THREE.MeshBasicMaterial({color: 0xff0000});
        const sphere = new THREE.Mesh(geometry, material);
        scene.add(sphere);
        camera.position.z = 5;

        // Add coordinate axes
        const axesHelper = new THREE.AxesHelper(5);
        scene.add(axesHelper);

        // Simulation parameters
        let params = {
            q: 1,
            E: 0,
            B: 1,
            m: 1,
            dt: 0.01,
            running: false
        };

        // Input listeners
        document.querySelectorAll('input[type="range"]').forEach(input => {
            input.addEventListener('input', (e) => {
                params[e.target.id] = parseFloat(e.target.value);
                document.getElementById(e.target.id + 'Value').textContent = e.target.value;
            });
        });

        function calculateAcceleration() {
            // Lorentz force: F = q(E + v × B)
            const E = new THREE.Vector3(params.E, 0, 0);
            const B = new THREE.Vector3(0, 0, params.B);
            
            const vCrossB = new THREE.Vector3().crossVectors(particle.velocity, B);
            const acceleration = E.add(vCrossB).multiplyScalar(params.q / params.m);
            
            return acceleration;
        }

        function updateParticle() {
            const acceleration = calculateAcceleration();
            particle.velocity.add(acceleration.multiplyScalar(params.dt));
            particle.position.add(particle.velocity.clone().multiplyScalar(params.dt));
            
            // Keep track of trail (last 100 positions)
            particle.trail.push(particle.position.clone());
            if(particle.trail.length > 100) particle.trail.shift();
        }

        function draw2D() {
            ctx.clearRect(0, 0, canvas2D.width, canvas2D.height);
            
            // Draw axes
            ctx.beginPath();
            ctx.moveTo(300, 0);
            ctx.lineTo(300, 400);
            ctx.moveTo(0, 200);
            ctx.lineTo(600, 200);
            ctx.strokeStyle = '#aaa';
            ctx.stroke();
            
            // Draw particle
            ctx.beginPath();
            ctx.arc(
                300 + particle.position.x * 30,
                200 - particle.position.y * 30,
                5, 0, Math.PI * 2
            );
            ctx.fillStyle = '#f00';
            ctx.fill();
            
            // Draw trail
            ctx.beginPath();
            particle.trail.forEach((pos, i) => {
                ctx.lineTo(
                    300 + pos.x * 30,
                    200 - pos.y * 30
                );
            });
            ctx.strokeStyle = 'rgba(255,0,0,0.3)';
            ctx.stroke();
        }

        function animate() {
            if(params.running) {
                updateParticle();
                draw2D();
                
                // Update 3D view
                sphere.position.copy(particle.position);
                camera.lookAt(particle.position);
                renderer.render(scene, camera);
            }
            requestAnimationFrame(animate);
        }

        function resetParticle() {
            particle.position.set(0, 0, 0);
            particle.velocity.set(params.v0, 0, 0);
            particle.trail = [];
        }

        function toggleAnimation() {
            params.running = !params.running;
        }

        // Initialize animation
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
