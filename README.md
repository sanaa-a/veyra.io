<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Veyra Prototype</title>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/loaders/STLLoader.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/controls/OrbitControls.js"></script>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', sans-serif;
        }

        body {
            overflow: hidden;
            background: linear-gradient(135deg, #e6f4ff, #f3eaff);
            color: #111;
        }

        .slide {
            width: 100vw;
            height: 100vh;
            display: none;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            text-align: center;
            padding: 40px;
        }

        .active {
            display: flex;
        }

        h1 {
            font-size: 55px;
            color: #6a5acd;
        }

        .split {
            display: flex;
            width: 100vw;
            height: 100vh;
        }

        .left {
            width: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #0a0a1a;
        }

        .right {
            width: 50%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 80px;
        }

        h2 {
            font-size: 40px;
            margin-bottom: 25px;
            color: #5a4fcf;
        }

        .text {
            font-size: 20px;
            line-height: 1.7;
        }

        .component {
            opacity: 0;
            transform: translateX(40px);
            margin: 15px 0;
            font-size: 22px;
            transition: all 0.6s ease;
        }

        .component.show {
            opacity: 1;
            transform: translateX(0);
        }

        nav {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
        }

        button {
            background: black;
            color: white;
            border: none;
            padding: 12px 22px;
            margin: 5px;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
        }

        button:hover {
            background: #6a5acd;
        }

        #viewer {
            width: 500px;
            height: 500px;
            position: relative;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 0 30px rgba(106, 90, 205, 0.5);
        }

        .hologram-badge {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: rgba(0, 255, 255, 0.3);
            color: cyan;
            padding: 8px 16px;
            border-radius: 30px;
            font-size: 14px;
            backdrop-filter: blur(5px);
            border: 1px solid cyan;
            z-index: 10;
            pointer-events: none;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% {
                opacity: 0.7;
                box-shadow: 0 0 5px cyan;
            }

            50% {
                opacity: 1;
                box-shadow: 0 0 20px cyan;
            }

            100% {
                opacity: 0.7;
                box-shadow: 0 0 5px cyan;
            }
        }
    </style>
</head>

<body>

    <!-- INTRO SLIDE -->
    <div class="slide active" id="intro">
        <h1>Veyra</h1>
        <p style="margin-top:20px;font-size:22px;">
            The New Holographic Video Calling Prototype
        </p>
    </div>

    <!-- MAIN CONTENT -->
    <div class="slide" id="content">
        <div class="split">
            <div class="left">
                <div id="viewer">
                    <div class="hologram-badge" id="hologramBadge">🌀 POV Effect Active</div>
                </div>
            </div>
            <div class="right">
                <h2 id="title"></h2>
                <div class="text" id="text"></div>
                <div id="componentsContainer"></div>
            </div>
        </div>
    </div>

    <nav>
        <button id="prevBtn" onclick="prevSlide()">Previous</button>
        <button id="nextBtn" onclick="nextSlide()">Next</button>
    </nav>

    <script>
        let current = 0;
        const slidesData = [
            { title: "About The Prototype", text: "The Veyra system uses rotating LED strips controlled by a microcontroller. Through Persistence of Vision, light slices blend into a floating 3D-like hologram." },
            { title: "How It Works", text: "Live video is processed into circular segments. LED strips rotate at high speed (1200+ RPM) to project a holographic image through Persistence of Vision. The spinning motion creates a floating 3D display visible from all angles.", hologram: true },
            { title: "Core Components", components: ["ESP32 Microcontroller", "6 High-Density LED Strips", "High-Speed Precision Motor (1200 RPM)", "Motor Driver Module", "Stable Power Supply", "Custom Video Processing Software"] },
            { title: "Key Features", text: "Real-time holographic visuals. Low-cost architecture. Scalable hardware design. Immersive presentation experience. Future AI integration ready." }
        ];

        const titleEl = document.getElementById("title");
        const textEl = document.getElementById("text");
        const compContainer = document.getElementById("componentsContainer");
        const hologramBadge = document.getElementById("hologramBadge");

        function showContent(index) {
            titleEl.innerHTML = slidesData[index].title;
            textEl.innerHTML = "";
            compContainer.innerHTML = "";

            if (slidesData[index].text) textEl.innerHTML = slidesData[index].text;

            if (slidesData[index].components) {
                slidesData[index].components.forEach((comp, i) => {
                    let div = document.createElement("div");
                    div.className = "component"; div.innerText = comp;
                    compContainer.appendChild(div);
                    setTimeout(() => div.classList.add("show"), 300 * i);
                });
            }

            if (slidesData[index].hologram) {
                startHologramAnimation();
                if (mesh) mesh.visible = false;
                if (fanGroup) fanGroup.visible = true;
                if (particleSystem) particleSystem.visible = true;
                hologramBadge.style.display = "block";
            } else {
                stopHologramAnimation();
                if (mesh) mesh.visible = true;
                if (fanGroup) fanGroup.visible = false;
                if (particleSystem) particleSystem.visible = false;
                hologramBadge.style.display = "none";
            }

            updateNavButtons();
        }

        function nextSlide() {
            if (current === 0) {
                document.getElementById("intro").classList.remove("active");
                document.getElementById("content").classList.add("active");
                current = 1;
                showContent(0);
            } else if (current < slidesData.length) {
                current++;
                showContent(current - 1);
            }
        }

        function prevSlide() {
            if (current === 1) {
                document.getElementById("content").classList.remove("active");
                document.getElementById("intro").classList.add("active");
                current = 0;
            } else if (current > 1) {
                current--;
                showContent(current - 1);
            }
        }

        function updateNavButtons() {
            document.getElementById("prevBtn").style.display = current === 0 ? "none" : "inline-block";
            document.getElementById("nextBtn").style.display = current === slidesData.length ? "none" : "inline-block";
        }

        /* ──────────────── THREE.JS ──────────────── */
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x050510); // deeper dark background

        const camera = new THREE.PerspectiveCamera(60, 1, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
        renderer.setSize(500, 500);
        document.getElementById("viewer").appendChild(renderer.domElement);

        // Add subtle fog for depth
        scene.fog = new THREE.FogExp2(0x050510, 0.002);

        scene.add(new THREE.AmbientLight(0x404060));
        const dirLight = new THREE.DirectionalLight(0xffffff, 1.2);
        dirLight.position.set(5, 10, 7);
        scene.add(dirLight);

        // Add a point light for glow effects
        const pointLight = new THREE.PointLight(0x4466ff, 1, 200);
        pointLight.position.set(0, 20, 30);
        scene.add(pointLight);

        let mesh;           // prototype STL
        let fanGroup;       // spinning LED arms
        let particleSystem; // floating particles for hologram effect
        let glowSphere;     // central glow

        let isHologramMode = false;
        let currentSpin = 0;
        const targetSpin = 0.85;   // faster spin for dramatic effect

        // Create a glowing central sphere
        glowSphere = new THREE.Mesh(
            new THREE.SphereGeometry(5, 32, 32),
            new THREE.MeshBasicMaterial({ color: 0x88aaff, transparent: true, opacity: 0.3 })
        );
        scene.add(glowSphere);

        // Load prototype STL
        const loader = new THREE.STLLoader();
        loader.load('prototype.stl', geometry => {
            const material = new THREE.MeshPhongMaterial({
                color: 0x8a7dff,
                shininess: 80,
                emissive: 0x221155,
                emissiveIntensity: 0.3
            });
            mesh = new THREE.Mesh(geometry, material);
            geometry.center();
            mesh.scale.set(1.8, 1.8, 1.8);
            mesh.rotation.set(0, 0, 0);
            mesh.position.y = -15;
            scene.add(mesh);
            camera.position.set(0, 20, 120);
            camera.lookAt(0, 0, 0);
        });

        // ── Enhanced holographic fan with LED strips and POV effect ──
        fanGroup = new THREE.Group();
        scene.add(fanGroup);
        fanGroup.visible = false;

        // Central hub with glow
        const hubGeo = new THREE.CylinderGeometry(8, 8, 10, 32);
        const hubMat = new THREE.MeshStandardMaterial({
            color: 0x222233,
            emissive: 0x224488,
            emissiveIntensity: 0.5
        });
        const hub = new THREE.Mesh(hubGeo, hubMat);
        hub.rotation.x = Math.PI / 2;
        fanGroup.add(hub);

        // Create 6 LED strips (more realistic for Veyra)
        const stripCount = 6;
        const stripLength = 100;
        const stripWidth = 1.5;
        const stripThickness = 0.8;

        // Different colors for each strip (rainbow effect)
        const stripColors = [
            0xff3366, // red-pink
            0xffaa33, // orange
            0xffff33, // yellow
            0x33ff33, // green
            0x33ccff, // light blue
            0xaa44ff  // purple
        ];

        for (let i = 0; i < stripCount; i++) {
            const angle = (i / stripCount) * Math.PI * 2;

            // Main LED strip (the physical arm)
            const armGeo = new THREE.BoxGeometry(stripWidth, stripLength, stripThickness);
            const armMat = new THREE.MeshStandardMaterial({
                color: 0x333344,
                emissive: 0x111122
            });
            const arm = new THREE.Mesh(armGeo, armMat);
            arm.position.set(
                Math.cos(angle) * (stripLength / 2 - 15),
                0,
                Math.sin(angle) * (stripLength / 2 - 15)
            );
            arm.rotation.y = angle;
            fanGroup.add(arm);

            // LED strip lights (the glowing part)
            // Create a series of small cubes along the arm to simulate individual LEDs
            const ledCount = 12;
            for (let j = 0; j < ledCount; j++) {
                const ledPos = (j / (ledCount - 1) - 0.5) * stripLength * 0.9;

                const ledGeo = new THREE.BoxGeometry(2.5, 1.2, 1.2);
                const ledMat = new THREE.MeshStandardMaterial({
                    color: stripColors[i % stripColors.length],
                    emissive: stripColors[i % stripColors.length],
                    emissiveIntensity: 0.8 + Math.sin(Date.now() * 0.01 + i + j) * 0.2
                });

                const led = new THREE.Mesh(ledGeo, ledMat);

                // Position along the arm
                led.position.set(
                    Math.cos(angle) * ledPos,
                    0,
                    Math.sin(angle) * ledPos
                );

                // Rotate to face outward
                led.rotation.y = angle;

                fanGroup.add(led);
            }
        }

        // Add light streaks to show rotation path
        const streakCount = 30;
        const streakGeo = new THREE.BufferGeometry();
        const streakPositions = [];
        const streakColors = [];

        for (let i = 0; i < streakCount; i++) {
            const radius = 40 + Math.random() * 30;
            const angle = Math.random() * Math.PI * 2;
            const height = (Math.random() - 0.5) * 40;

            // Starting point
            streakPositions.push(
                Math.cos(angle) * radius,
                height,
                Math.sin(angle) * radius
            );

            // Ending point (slightly further along the circle)
            streakPositions.push(
                Math.cos(angle + 0.2) * radius,
                height,
                Math.sin(angle + 0.2) * radius
            );

            // Color based on angle
            const color = new THREE.Color().setHSL(angle / (Math.PI * 2), 0.8, 0.5);
            streakColors.push(color.r, color.g, color.b);
            streakColors.push(color.r, color.g, color.b);
        }

        streakGeo.setAttribute('position', new THREE.Float32BufferAttribute(streakPositions, 3));
        streakGeo.setAttribute('color', new THREE.Float32BufferAttribute(streakColors, 3));

        const streakMat = new THREE.LineBasicMaterial({ vertexColors: true, transparent: true, opacity: 0.15 });
        const streaks = new THREE.LineSegments(streakGeo, streakMat);
        fanGroup.add(streaks);

        // Particle system for holographic "dust"
        const particleCount = 300;
        const particleGeo = new THREE.BufferGeometry();
        const particlePositions = new Float32Array(particleCount * 3);
        const particleColors = new Float32Array(particleCount * 3);

        for (let i = 0; i < particleCount; i++) {
            // Random positions in a cylinder volume
            const r = 20 + Math.random() * 60;
            const theta = Math.random() * Math.PI * 2;
            const y = (Math.random() - 0.5) * 70;

            particlePositions[i * 3] = Math.cos(theta) * r;
            particlePositions[i * 3 + 1] = y;
            particlePositions[i * 3 + 2] = Math.sin(theta) * r;

            // Color based on height
            const hue = (y / 70 + 1) * 0.3 + 0.5;
            const color = new THREE.Color().setHSL(hue, 0.9, 0.6);
            particleColors[i * 3] = color.r;
            particleColors[i * 3 + 1] = color.g;
            particleColors[i * 3 + 2] = color.b;
        }

        particleGeo.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));
        particleGeo.setAttribute('color', new THREE.BufferAttribute(particleColors, 3));

        const particleMat = new THREE.PointsMaterial({
            size: 1.5,
            vertexColors: true,
            transparent: true,
            opacity: 0.4,
            blending: THREE.AdditiveBlending
        });
        particleSystem = new THREE.Points(particleGeo, particleMat);
        scene.add(particleSystem);
        particleSystem.visible = false;

        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.minDistance = 40;
        controls.maxDistance = 180;
        controls.autoRotate = false;
        controls.enableZoom = true;

        function startHologramAnimation() {
            isHologramMode = true;
        }

        function stopHologramAnimation() {
            isHologramMode = false;
            currentSpin = 0;
            fanGroup.rotation.y = 0;
        }

        // Animation variables for LED pulsing
        let time = 0;

        function animate() {
            requestAnimationFrame(animate);

            time += 0.01;

            // Animate LED strips (pulsing glow)
            if (fanGroup && fanGroup.children) {
                fanGroup.children.forEach((child, index) => {
                    // Only affect the LED cubes (small boxes with emissive material)
                    if (child.geometry && child.geometry.parameters &&
                        child.geometry.parameters.width === 2.5 &&
                        child.material && child.material.emissive) {
                        // Pulse intensity based on time and position
                        const pulse = 0.7 + Math.sin(time * 5 + index * 0.5) * 0.3;
                        if (child.material.emissiveIntensity !== undefined) {
                            child.material.emissiveIntensity = pulse;
                        }
                    }
                });
            }

            if (isHologramMode) {
                // Faster spin for dramatic effect
                currentSpin = THREE.MathUtils.lerp(currentSpin, targetSpin, 0.15);
                fanGroup.rotation.y += currentSpin;

                // Make particles rotate slowly too (for trailing effect)
                if (particleSystem) {
                    particleSystem.rotation.y += 0.001;
                }

                // Pulsing central glow
                if (glowSphere) {
                    glowSphere.scale.setScalar(1 + Math.sin(time * 10) * 0.1);
                }
            } else {
                currentSpin *= 0.9;
                fanGroup.rotation.y += currentSpin;
            }

            controls.update();
            renderer.render(scene, camera);
        }
        animate();
        updateNavButtons();

        // Initial badge state
        hologramBadge.style.display = "none";
    </script>

</body>

</html>
