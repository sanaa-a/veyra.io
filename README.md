<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Veyra · volumetric POV · pastel edition</title>
    <!-- Font & smoothness -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background: #f9f7ff;
            color: #1b1b2f;
            min-height: 100vh;
            overflow: hidden;
        }

        .slide {
            position: absolute;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            display: flex;
            opacity: 0;
            transition: opacity 0.7s cubic-bezier(0.2, 0.9, 0.3, 1);
            pointer-events: none;
        }

        .slide.active {
            opacity: 1;
            pointer-events: all;
        }

        .left-panel {
            width: 50%;
            height: 100vh;
            background: linear-gradient(145deg, #ffffff 0%, #f0ecff 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 4rem 5rem;
            position: relative;
            overflow: hidden;
        }

        .right-panel {
            width: 50%;
            height: 100vh;
            background: linear-gradient(135deg, #ede9ff 0%, #faf9ff 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .content {
            max-width: 550px;
        }

        .slide-title {
            font-size: 3.8rem;
            font-weight: 700;
            background: linear-gradient(130deg, #5b6ee8, #a58bff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 1.25rem;
            line-height: 1.2;
            letter-spacing: -0.02em;
            transform: translateY(30px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.2, 0.8, 0.3, 1) 0.15s;
        }

        .slide-text {
            font-size: 1.25rem;
            line-height: 1.7;
            color: #2d2a4a;
            margin-bottom: 2.5rem;
            font-weight: 400;
            transform: translateY(30px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.2, 0.8, 0.3, 1) 0.25s;
        }

        .component-item {
            padding: 1.1rem 1.5rem;
            margin-bottom: 0.8rem;
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(4px);
            border-radius: 24px;
            border: 1px solid rgba(173, 157, 255, 0.3);
            box-shadow: 0 10px 25px -8px rgba(130, 100, 240, 0.15);
            transform: translateX(30px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.15, 0.85, 0.25, 1);
            font-size: 1.1rem;
            font-weight: 500;
            color: #201e3a;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .component-item::before {
            content: "✦";
            font-size: 1.5rem;
            background: linear-gradient(140deg, #5f7ef2, #b18cff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-weight: 300;
        }

        .component-item:nth-child(1) { transition-delay: 0.1s; }
        .component-item:nth-child(2) { transition-delay: 0.2s; }
        .component-item:nth-child(3) { transition-delay: 0.3s; }
        .component-item:nth-child(4) { transition-delay: 0.4s; }
        .component-item:nth-child(5) { transition-delay: 0.5s; }
        .component-item:nth-child(6) { transition-delay: 0.6s; }

        .slide.active .slide-title,
        .slide.active .slide-text,
        .slide.active .component-item {
            transform: translateY(0) translateX(0);
            opacity: 1;
        }

        .visualization-container {
            width: 500px;
            height: 500px;
            position: relative;
            background: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(6px);
            border-radius: 48px;
            box-shadow: 0 35px 60px -15px rgba(120, 100, 210, 0.3);
            overflow: hidden;
        }

        .phase-indicator {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 14px;
            z-index: 100;
        }

        .phase-dot {
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background: #d6ceff;
            border: 2px solid rgba(255,255,255,0.6);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .phase-dot.active {
            background: linear-gradient(140deg, #6a5ef0, #a185ff);
            transform: scale(1.4);
            border-color: white;
            box-shadow: 0 0 18px #b29eff;
        }

        .nav-buttons {
            position: fixed;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
            z-index: 200;
        }

        .nav-btn {
            padding: 14px 40px;
            background: white;
            color: #2a2745;
            border: none;
            border-radius: 60px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.25s ease;
            box-shadow: 0 15px 30px -12px rgba(130, 100, 240, 0.35);
            letter-spacing: 0.3px;
            background: rgba(255, 255, 255, 0.9);
        }

        .nav-btn:hover:not(:disabled) {
            background: white;
            transform: translateY(-4px);
            box-shadow: 0 25px 35px -12px #9b89ff;
            color: #3d34b0;
        }

        .nav-btn:disabled {
            background: rgba(230, 225, 250, 0.5);
            color: #8a85b0;
            cursor: not-allowed;
        }

        .badge {
            position: absolute;
            top: 20px;
            right: 20px;
            background: linear-gradient(120deg, #ffffffcc, #edeaffcc);
            backdrop-filter: blur(10px);
            color: #3d3396;
            padding: 8px 20px;
            border-radius: 60px;
            font-size: 0.9rem;
            font-weight: 600;
            border: 1px solid rgba(255,255,255,0.9);
            z-index: 50;
        }

        .hologram-overlay {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 220px;
            height: 220px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(185, 165, 255, 0.25) 0%, transparent 70%);
            pointer-events: none;
            z-index: 10;
        }
    </style>
</head>
<body>
    <!-- SLIDE 1 intro -->
    <div class="slide active" id="slide1">
        <div class="left-panel">
            <div class="content">
                <h1 class="slide-title">Veyra</h1>
                <p class="slide-text">Step into volumetric presence — where light weaves dimension. Our POV display paints the air with pastel holography.</p>
                <div style="margin-top: 40px;">
                    <div class="component-item">✨ real‑time 3D holographic projection</div>
                    <div class="component-item">🌀 Persistence of Vision (POV) 2.0</div>
                    <div class="component-item">📡 low‑latency volumetric stream</div>
                </div>
            </div>
        </div>
        <div class="right-panel">
            <div class="visualization-container">
                <div id="viewer1"></div>
                <div class="badge">pastel · POV</div>
                <div class="hologram-overlay"></div>
            </div>
        </div>
    </div>

    <!-- SLIDE 2 how it works -->
    <div class="slide" id="slide2">
        <div class="left-panel">
            <div class="content">
                <h1 class="slide-title">How it works</h1>
                <p class="slide-text">Five blades, one silent motor. At 1200 RPM, LEDs flicker faster than the eye — a floating volume appears.</p>
                <div style="margin-top: 40px;">
                    <div class="component-item">⚡ 5‑blade LED fan (144 LEDs/m)</div>
                    <div class="component-item">🔄 1200 RPM brushless motor</div>
                    <div class="component-item">💡 RGB pastel‑matrix strips</div>
                    <div class="component-item">🧠 ESP32‑S3 POV engine</div>
                </div>
            </div>
        </div>
        <div class="right-panel">
            <div class="visualization-container">
                <div id="viewer2"></div>
                <div class="badge">🌀 20ms persistence</div>
                <div class="hologram-overlay"></div>
            </div>
        </div>
    </div>

    <!-- SLIDE 3 components -->
    <div class="slide" id="slide3">
        <div class="left-panel">
            <div class="content">
                <h1 class="slide-title">Core components</h1>
                <div style="margin-top: 30px;">
                    <div class="component-item">🔷 ESP32‑P4 (dual‑core)</div>
                    <div class="component-item">💡 6x high‑density LED strips</div>
                    <div class="component-item">⚙️ silent 1400 RPM motor</div>
                    <div class="component-item">🔋 power delivery module (12V)</div>
                    <div class="component-item">🔌 magnetic hall sensor</div>
                    <div class="component-item">📹 WebRTC video shader</div>
                </div>
            </div>
        </div>
        <div class="right-panel">
            <div class="visualization-container">
                <div id="viewer3"></div>
                <div class="badge">premium · whisper quiet</div>
                <div class="hologram-overlay"></div>
            </div>
        </div>
    </div>

    <!-- SLIDE 4 features -->
    <div class="slide" id="slide4">
        <div class="left-panel">
            <div class="content">
                <h1 class="slide-title">Key features</h1>
                <p class="slide-text">Real‑time 3D, low‑cost, open‑source. Designed for telepresence, art, and future AI avatars.</p>
                <div style="margin-top: 40px;">
                    <div class="component-item">📺 true 3D without glasses</div>
                    <div class="component-item">💰 1/10 cost of laser holography</div>
                    <div class="component-item">🔌 plug‑and‑play volumetric</div>
                    <div class="component-item">🌐 ready for remote collaboration</div>
                </div>
            </div>
        </div>
        <div class="right-panel">
            <div class="visualization-container">
                <div id="viewer4"></div>
                <div class="badge">AI‑ready · 2025</div>
                <div class="hologram-overlay"></div>
            </div>
        </div>
    </div>

    <!-- Navigation & dots -->
    <div class="nav-buttons">
        <button class="nav-btn" id="prevBtn" onclick="prevSlide()" disabled>← Previous</button>
        <button class="nav-btn" id="nextBtn" onclick="nextSlide()">Next →</button>
    </div>
    <div class="phase-indicator">
        <div class="phase-dot active" data-slide="0"></div>
        <div class="phase-dot" data-slide="1"></div>
        <div class="phase-dot" data-slide="2"></div>
        <div class="phase-dot" data-slide="3"></div>
    </div>

    <!-- Load Three.js and OrbitControls in correct order -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>

    <script>
        // slide navigation
        let currentSlide = 0;
        const slides = document.querySelectorAll('.slide');
        const dots = document.querySelectorAll('.phase-dot');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');

        // viewers
        const viewers = [
            document.getElementById('viewer1'),
            document.getElementById('viewer2'),
            document.getElementById('viewer3'),
            document.getElementById('viewer4')
        ];

        const scenes = [], renderers = [], fanGroups = [], cameras = [];

        // initialize all four 3d viewers
        viewers.forEach((container, idx) => {
            const scene = new THREE.Scene();
            scene.background = new THREE.Color(0xf8f4ff);

            const camera = new THREE.PerspectiveCamera(55, 1, 0.1, 1000);
            camera.position.set(0, 15, 100);  // Adjusted for better view
            camera.lookAt(0, 0, 0);

            const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
            renderer.setSize(500, 500);
            renderer.setClearColor(0xf8f4ff);
            container.appendChild(renderer.domElement);

            // lighting
            const ambient = new THREE.AmbientLight(0xbfb5ff);
            scene.add(ambient);

            const dirLight = new THREE.DirectionalLight(0xfff2fc, 1.2);
            dirLight.position.set(8, 15, 12);
            scene.add(dirLight);

            const backLight = new THREE.PointLight(0xb5a6ff, 0.8);
            backLight.position.set(-10, 5, -20);
            scene.add(backLight);

            // fan group
            const fanGroup = new THREE.Group();
            scene.add(fanGroup);

            // central hub
            const hubGeo = new THREE.CylinderGeometry(11, 11, 12, 32);
            const hubMat = new THREE.MeshStandardMaterial({ color: 0x9b8cff, emissive: 0x332a55, emissiveIntensity: 0.3 });
            const hub = new THREE.Mesh(hubGeo, hubMat);
            hub.rotation.x = Math.PI / 2;
            fanGroup.add(hub);

            const armCount = 5;
            const armLength = 100;

            for (let i = 0; i < armCount; i++) {
                const angle = (i / armCount) * Math.PI * 2;

                // arm
                const armGeo = new THREE.BoxGeometry(2.2, armLength, 2.2);
                const armMat = new THREE.MeshStandardMaterial({ color: 0xd9d0ff, emissive: 0x1e1933, transparent: true, opacity: 0.5 });
                const arm = new THREE.Mesh(armGeo, armMat);
                arm.position.set(Math.cos(angle) * (armLength/2 + 5), 0, Math.sin(angle) * (armLength/2 + 5));
                arm.rotation.y = angle;
                fanGroup.add(arm);

                // LEDs
                const ledCount = 24;
                for (let j = 0; j < ledCount; j++) {
                    const t = j / (ledCount - 1);
                    const pos = (t - 0.5) * armLength * 0.9;
                    
                    // Create gradient from blue to purple
                    const hue = 0.65 + t * 0.25;
                    const color = new THREE.Color().setHSL(hue % 1.0, 0.8, 0.75);

                    const ledGeo = new THREE.BoxGeometry(2.2, 1.8, 1.8);
                    const ledMat = new THREE.MeshStandardMaterial({
                        color: color,
                        emissive: color,
                        emissiveIntensity: 0.9
                    });
                    const led = new THREE.Mesh(ledGeo, ledMat);
                    led.position.set(
                        Math.cos(angle) * pos,
                        0,
                        Math.sin(angle) * pos
                    );
                    led.rotation.y = angle;
                    fanGroup.add(led);
                }

                // tip LED
                const tipGeo = new THREE.BoxGeometry(3.5, 3, 3);
                const tipMat = new THREE.MeshStandardMaterial({
                    color: 0xd4e0ff,
                    emissive: 0xaabbff,
                    emissiveIntensity: 1.2
                });
                const tip = new THREE.Mesh(tipGeo, tipMat);
                tip.position.set(
                    Math.cos(angle) * (armLength/2 + 5),
                    0,
                    Math.sin(angle) * (armLength/2 + 5)
                );
                tip.rotation.y = angle;
                fanGroup.add(tip);
            }

            scenes[idx] = scene;
            renderers[idx] = renderer;
            fanGroups[idx] = fanGroup;
            cameras[idx] = camera;
        });

        // Simple animation loop - NO OrbitControls to interfere with spinning
        function animate() {
            requestAnimationFrame(animate);
            
            fanGroups.forEach((fan, idx) => {
                if (!fan) return;
                
                // Different speeds per slide - like your original code
                if (idx === 1) { // How it works - fastest
                    fan.rotation.y += 0.95;
                } else if (idx === 3) { // Features - medium-fast
                    fan.rotation.y += 0.85;
                } else if (idx === 2) { // Components - medium
                    fan.rotation.y += 0.35;
                } else { // Intro - slower
                    fan.rotation.y += 0.15;
                }

                if (renderers[idx] && scenes[idx] && cameras[idx]) {
                    renderers[idx].render(scenes[idx], cameras[idx]);
                }
            });
        }
        
        // Start animation
        animate();

        // Navigation functions
        function nextSlide() {
            if (currentSlide < slides.length - 1) {
                slides[currentSlide].classList.remove('active');
                dots[currentSlide].classList.remove('active');
                currentSlide++;
                slides[currentSlide].classList.add('active');
                dots[currentSlide].classList.add('active');
                updateNavButtons();
            }
        }

        function prevSlide() {
            if (currentSlide > 0) {
                slides[currentSlide].classList.remove('active');
                dots[currentSlide].classList.remove('active');
                currentSlide--;
                slides[currentSlide].classList.add('active');
                dots[currentSlide].classList.add('active');
                updateNavButtons();
            }
        }

        function updateNavButtons() {
            prevBtn.disabled = currentSlide === 0;
            nextBtn.disabled = currentSlide === slides.length - 1;
        }

        // Dot click navigation
        dots.forEach((dot, index) => {
            dot.addEventListener('click', () => {
                if (index === currentSlide) return;
                slides[currentSlide].classList.remove('active');
                dots[currentSlide].classList.remove('active');
                currentSlide = index;
                slides[currentSlide].classList.add('active');
                dots[currentSlide].classList.add('active');
                updateNavButtons();
            });
        });
    </script>
</body>
</html>
