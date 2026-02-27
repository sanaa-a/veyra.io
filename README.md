<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Veyra's Prototype</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(145deg, #2F2A4A 0%, #1E193A 100%);
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1.5rem;
            overflow: hidden;
        }

        .slider-container {
            width: 100%;
            max-width: 1600px;
            height: 100vh;
            background: rgba(205, 186, 243, 0.55);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-radius: 60px;
            box-shadow: 0 30px 50px -20px #00000080, 0 0 0 1.5px #FFFFFF30 inset;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            border: 1px solid #FFFFFF40;
        }

        /* header with balanced logo */
        .slide-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 1rem 2.2rem;
            background: rgba(111, 81, 247, 0.45);
            border-bottom: 1px solid #FFFFFF30;
            flex-wrap: wrap;
        }

        .logo-area {
            display: flex;
            align-items: center;
            gap: 0.65rem;
        }

        .logo-icon {
            background: linear-gradient(145deg, #b4a6ff, #7f66cc);
            border-radius: 10px;
            width: 38px;
            height: 38px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 12px #22006670;
            font-size: 1.7rem;
            color: white;
            font-weight: 500;
        }

        .logo-text {
            font-size: 1.5rem;
            font-weight: 530;
            color: #FFFFFF;
            letter-spacing: -0.02em;
            text-shadow: 0 2px 5px #00000050;
        }

        .slide-controls {
            display: flex;
            gap: 1rem;
            align-items: center;
        }

        .slide-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1.5px solid #FFFFFF50;
            color: #FFFFFF;
            font-size: 1rem;
            padding: 0.5rem 1.5rem;
            border-radius: 40px;
            font-weight: 500;
            cursor: pointer;
            backdrop-filter: blur(10px);
            transition: all 0.2s;
            box-shadow: 0 4px 12px #00000040;
        }

        .slide-btn:hover {
            background: rgba(244, 237, 237, 0.863);
            border-color: #FFFFFF90;
            transform: translateY(-2px);
            box-shadow: 0 8px 18px #00000060;
        }

        .slide-indicator {
            display: flex;
            gap: 10px;
            margin-left: 1rem;
        }

        .dot {
            width: 10px;
            height: 10px;
            border-radius: 20px;
            background: #FFFFFF40;
            transition: all 0.3s;
        }

        .dot.active {
            width: 34px;
            background: #FFFFFF;
            box-shadow: 0 0 15px #AA99FF;
        }

        .slides-wrapper {
            flex: 1;
            display: flex;
            overflow-x: auto;
            scroll-snap-type: x mandatory;
            scroll-behavior: smooth;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
        }

        .slides-wrapper::-webkit-scrollbar {
            display: none;
        }

        .slide {
            flex: 0 0 100%;
            scroll-snap-align: start;
            padding: 2rem 2.2rem;
            display: flex;
            gap: 2rem;
            overflow-y: auto;
        }

        /* left column */
        .prototype-column {
            flex: 1;
            background: #e4dcfa;
            border-radius: 48px;
            padding: 2rem;
            border: 2px solid #FFFFFF30;
            box-shadow: 0 20px 30px -12px #000000, inset 0 0 30px #000000;
            display: flex;
            flex-direction: column;
        }

        .prototype-title {
            color: #5850c9;
            font-size: 1.8rem;
            font-weight: 500;
            margin-bottom: 1.2rem;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }

        .prototype-title span {
            background: #AA99FF30;
            padding: 0.3rem 1.2rem;
            border-radius: 40px;
            font-size: 1rem;
            border: 1px solid #AA99FF;
        }

        .visual-container {
            flex: 1;
            position: relative;
            background: #8383e7e3;
            border-radius: 42px;
            overflow: hidden;
            border: 1px solid #FFFFFF30;
            box-shadow: 0 0 30px #000000 inset;
            min-height: 300px;
        }

        /* fan parts */
        .stand {
            position: absolute;
            bottom: 15px;
            left: 50%;
            transform: translateX(-50%);
            width: 24px;
            height: 150px;
            background: linear-gradient(to right, #444, #666, #444);
            border-radius: 30px 30px 8px 8px;
            box-shadow: 0 10px 15px #000;
            z-index: 10;
        }

        .motor {
            position: absolute;
            bottom: 160px;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 42px;
            background: #333;
            border-radius: 50px;
            box-shadow: 0 8px 0 #1A1A1A;
            z-index: 15;
            transition: box-shadow 0.3s ease;
        }

        .fan-hub {
            position: absolute;
            bottom: 198px;
            left: 50%;
            transform: translateX(-50%);
            width: 76px;
            height: 76px;
            background: #222;
            border-radius: 50%;
            box-shadow: 0 0 0 4px #FFFFFF20;
            z-index: 20;
        }

        .fan-blade {
            position: absolute;
            bottom: 236px;
            left: 50%;
            transform-origin: bottom center;
            width: 8px;
            height: 180px;
            background: linear-gradient(to top, #AAA, #EEE, #AAA);
            border-radius: 30px 30px 0 0;
            box-shadow: 0 0 15px #FFFFFF60;
            z-index: 6;
            transition: box-shadow 0.3s ease, background 0.3s ease;
        }

        .camera {
            position: absolute;
            top: 25px;
            right: 25px;
            width: 90px;
            height: 65px;
            background: #3d3b3bec;
            border-radius: 30px 30px 18px 18px;
            border: 2px solid #FFFFFF40;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 50;
            transition: box-shadow 0.3s ease;
        }

        .camera-lens {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: radial-gradient(circle, #4a05056b, #848383);
            border: 1px solid #FFFFFF60;
            transition: box-shadow 0.3s ease;
        }

        /* glow utilities */
        .glow-motor {
            box-shadow: 0 0 30px #FFF, 0 8px 0 #1A1A1A !important;
        }

        .glow-blade {
            box-shadow: 0 0 30px #FFF !important;
            background: linear-gradient(to top, #FFF, #FFF, #AAA) !important;
        }

        .glow-camera {
            box-shadow: 0 0 30px #FFF !important;
        }

        .glow-lens {
            box-shadow: 0 0 30px #FFF !important;
        }

        .glow-hub {
            box-shadow: 0 0 30px #FFF !important;
        }

        .image-output {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: radial-gradient(circle, #FFFFFF40, #FFFFFF10);
            opacity: 0;
            z-index: 30;
        }

        .pov-effect {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 260px;
            height: 260px;
            border-radius: 50%;
            background: conic-gradient(from 0deg, #FFAAFF30, #AAAAFF30, #FFAAFF30, #AAAAFF30, #FFAAFF30);
            opacity: 0;
            z-index: 25;
            mix-blend-mode: screen;
        }

        /* ---------- IMAGE HOLOGRAM - NO BORDER, NO BACKGROUND, JUST PNG ---------- */
        .hologram-image-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 220px;
            /* comfortable size for man.png */
            height: auto;
            /* height auto to preserve aspect */
            max-height: 280px;
            z-index: 40;
            opacity: 0;
            /* hidden by default, shown via glow / step 4 */
            transition: opacity 0.35s ease, filter 0.3s;
            background: none;
            /* explicitly no background */
            border: none;
            /* no border */
            box-shadow: none;
            /* no box shadow – if you want glow only from white glow class */
            display: flex;
            align-items: center;
            justify-content: center;
            pointer-events: none;
            /* image doesn't block interaction */
        }

        .hologram-image-container img {
            width: 100%;
            height: auto;
            max-height: 280px;
            object-fit: contain;
            /* keeps aspect ratio, no cropping */
            display: block;
            background: transparent;
            /* ensure no background */
            border: none;
            filter: drop-shadow(0 0 15px rgba(255, 255, 255, 0.3));
            /* subtle glow, but removed on step5 stronger */
        }

        /* optional small caption – but we can keep it minimal */
        .image-caption {
            position: absolute;
            bottom: -20px;
            right: 0;
            background: none;
            color: rgba(255, 255, 255, 0.7);
            font-size: 0.7rem;
            padding: 2px 8px;
            border: none;
            opacity: 0.6;
            white-space: nowrap;
            text-shadow: 0 0 5px black;
        }

        /* right column */
        .content-column {
            flex: 1;
            background: rgba(15, 10, 25, 0.7);
            backdrop-filter: blur(16px);
            border-radius: 48px;
            padding: 2rem;
            border: 2px solid #FFFFFF30;
            color: #FFF;
            overflow-y: auto;
            scrollbar-width: thin;
            scrollbar-color: #AA99FF #1A1A2A;
        }

        .content-column::-webkit-scrollbar {
            width: 6px;
        }

        .content-column::-webkit-scrollbar-track {
            background: #59599c;
            border-radius: 20px;
        }

        .content-column::-webkit-scrollbar-thumb {
            background: #AA99FF;
            border-radius: 20px;
        }

        .slide-title {
            font-size: 2.2rem;
            font-weight: 600;
            margin-bottom: 1rem;
            border-bottom: 3px solid #AA99FF;
            display: inline-block;
            padding-bottom: 0.3rem;
        }

        .intro-text {
            font-size: 1.3rem;
            line-height: 1.6;
            color: #DDD;
            margin: 1.5rem 0;
        }

        .highlight-box {
            background: #aa99ff92;
            border: 1px solid #AA99FF;
            border-radius: 32px;
            padding: 1.5rem;
            margin-top: 2rem;
        }

        .components-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1rem;
            margin-top: 1.5rem;
        }

        .comp-card {
            background: #ffffff68;
            border: 1px solid #ffffff9c;
            border-radius: 28px;
            padding: 1.2rem;
            display: flex;
            align-items: center;
            gap: 1rem;
            backdrop-filter: blur(5px);
        }

        .comp-icon {
            width: 48px;
            height: 48px;
            background: #aa99ffa4;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
        }

        /* steps */
        .steps-container {
            margin: 1.5rem 0;
        }

        .step-row {
            display: flex;
            align-items: center;
            gap: 1.2rem;
            background: #9b9af5d9;
            border-radius: 50px;
            padding: 1rem 1.5rem;
            margin-bottom: 1rem;
            border: 1px solid #FFFFFF30;
            transition: all 0.3s;
        }

        .step-row.active-step {
            background: #aa99ff99;
            border-color: #FFF;
            box-shadow: 0 0 20px #AA99FF;
        }

        .step-num {
            width: 40px;
            height: 40px;
            background: #AA99FF;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            color: #000;
        }

        .step-info {
            flex: 1;
        }

        .step-title {
            font-weight: 600;
            font-size: 1.2rem;
        }

        .step-desc {
            color: #CCC;
            font-size: 0.95rem;
        }

        .auto-badge {
            background: #AA99FF;
            color: #000;
            padding: 0.3rem 1.2rem;
            border-radius: 30px;
            font-weight: 600;
            font-size: 0.9rem;
            margin-left: 1rem;
        }

        .uses-grid {
            display: grid;
            gap: 1.2rem;
            margin-top: 1.5rem;
        }

        .use-card {
            background: #ffffff81;
            border: 1px solid #FFFFFF30;
            border-radius: 32px;
            padding: 1.5rem;
        }

        .use-card h4 {
            font-size: 1.5rem;
            color: #FFF;
            margin-bottom: 0.3rem;
        }

        @keyframes rotateBlade {
            from {
                transform: translateX(-50%) rotate(0deg);
            }

            to {
                transform: translateX(-50%) rotate(360deg);
            }
        }

        .rotating {
            animation: rotateBlade 1.2s linear infinite !important;
        }
    </style>
</head>

<body>
    <div class="slider-container">
        <div class="slide-header">
            <div class="logo-area">
                <div class="logo-icon"><span>◈</span></div>
                <div class="logo-text">Veyra</div>
            </div>
            <div class="slide-controls">
                <button class="slide-btn" id="prevSlide">← Previous</button>
                <div class="slide-indicator" id="slideDots">
                    <span class="dot active"></span><span class="dot"></span><span class="dot"></span><span
                        class="dot"></span>
                </div>
                <button class="slide-btn" id="nextSlide">Next →</button>
            </div>
        </div>

        <div class="slides-wrapper" id="slidesWrapper">
            <!-- SLIDE 1 : intro with man.png (transparent background) -->
            <div class="slide" id="slide1">
                <div class="prototype-column">
                    <div class="prototype-title">6-Veyra's Prototype <span></span></div>
                    <div class="visual-container" id="visual1">
                        <div class="stand"></div>
                        <div class="motor" id="motor1"></div>
                        <div class="fan-hub" id="hub1"></div>
                        <div class="camera" id="camera1">
                            <div class="camera-lens" id="lens1"></div>
                        </div>
                        <div class="image-output" id="output1"></div>
                        <div class="pov-effect" id="pov1"></div>
                        <!-- man.png with no border, no background -->
                        <div class="hologram-image-container" id="holoImg1">
                            <img src="man.png"
                                onerror="this.src='https://placehold.co/400x480/2c1e4a/AA99FF?text=man.png'">
                            <span </span>
                        </div>
                    </div>
                </div>
                <div class="content-column">
                    <div class="slide-title">Introduction</div>
                    <div class="intro-text">Hologram Projection For Video Calling</div>
                    <p style="font-size:1.1rem; margin:1rem 0;">A high-speed persistence-of-vision (POV) display using
                        six rotating blades with RGB LEDs. Creates floating 3D images perceived as a hologram.</p>
                    <div class="highlight-box">
                        <p style="font-weight:600;">✦ 6 Blades for smoother POV</p>
                        <p style="font-weight:600;">✦ Real-time 3D capture & display</p>
                    </div>
                </div>
            </div>
            <!-- SLIDE 2 : components with man.png -->
            <div class="slide" id="slide2">
                <div class="prototype-column">
                    <div class="prototype-title">Component View <span>6-Blade</span></div>
                    <div class="visual-container" id="visual2">
                        <div class="stand"></div>
                        <div class="motor" id="motor2"></div>
                        <div class="fan-hub" id="hub2"></div>
                        <div class="camera" id="camera2">
                            <div class="camera-lens" id="lens2"></div>
                        </div>
                        <div class="image-output" id="output2"></div>
                        <div class="pov-effect" id="pov2"></div>
                        <div class="hologram-image-container" id="holoImg2">
                            <img src="man.png" alt="man components"
                                onerror="this.src='https://placehold.co/400x480/2c1e4a/AA99FF?text=man.png'">

                        </div>
                    </div>
                </div>
                <div class="content-column">
                    <div class="slide-title">Key Components</div>
                    <div class="components-grid">
                        <div class="comp-card"><span class="comp-icon">🧠</span> ESP32</div>
                        <div class="comp-card"><span class="comp-icon">🔄</span> High-Speed Motor</div>
                        <div class="comp-card"><span class="comp-icon">🌀</span> 6 RGB LED Blades</div>
                        <div class="comp-card"><span class="comp-icon">📷</span> 4K Camera</div>
                    </div>
                </div>
            </div>
            <!-- SLIDE 3 : How it works with man.png (no border/background) -->
            <div class="slide" id="slide3">
                <div class="prototype-column">
                    <div class="prototype-title">Active Sequence <span>Auto-Run</span></div>
                    <div class="visual-container" id="visual3">
                        <div class="stand"></div>
                        <div class="motor" id="motor3"></div>
                        <div class="fan-hub" id="hub3"></div>
                        <div class="camera" id="camera3">
                            <div class="camera-lens" id="lens3"></div>
                        </div>
                        <div class="image-output" id="output3"></div>
                        <div class="pov-effect" id="pov3"></div>
                        <!-- man.png pure transparent container -->
                        <div class="hologram-image-container" id="holoImg3">
                            <img src="man.png" alt="hologram man" id="holoImageTag3"
                                onerror="this.src='https://placehold.co/400x480/2c1e4a/AA99FF?text=man.png'">
                            <span class="image-caption">man.png</span>
                        </div>
                    </div>
                </div>
                <div class="content-column">
                    <div class="slide-title">How It Works</div>
                    <p style="margin-bottom: 1rem; color: #b9b3ff;">The demo of the prototype will be auto-played.
                        Please wait till it completes!</p>
                    <div class="steps-container" id="stepsContainer">
                        <div class="step-row" id="step1Row">
                            <div class="step-num">1</div>
                            <div class="step-info"><span class="step-title">Assembly</span>
                                <div class="step-desc">Motor, 6-blade fan, camera</div>
                            </div><span class="auto-badge">ACTIVE</span>
                        </div>
                        <div class="step-row" id="step2Row">
                            <div class="step-num">2</div>
                            <div class="step-info"><span class="step-title">Calibration</span>
                                <div class="step-desc">Camera calibration for 3D mapping</div>
                            </div>
                        </div>
                        <div class="step-row" id="step3Row">
                            <div class="step-num">3</div>
                            <div class="step-info"><span class="step-title">Capture</span>
                                <div class="step-desc">Multi-angle 2D capture</div>
                            </div>
                        </div>
                        <div class="step-row" id="step4Row">
                            <div class="step-num">4</div>
                            <div class="step-info"><span class="step-title">Processing</span>
                                <div class="step-desc">2D → 3D conversion</div>
                            </div>
                        </div>
                        <div class="step-row" id="step5Row">
                            <div class="step-num">5</div>
                            <div class="step-info"><span class="step-title">POV Display</span>
                                <div class="step-desc">Spinning fan generates 3D hologram</div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
            <!-- SLIDE 4 : Applications with man.png -->
            <div class="slide" id="slide4">
                <div class="prototype-column">
                    <div class="prototype-title">Applications <span>Use Cases</span></div>
                    <div class="visual-container" id="visual4">
                        <div class="stand"></div>
                        <div class="motor" id="motor4"></div>
                        <div class="fan-hub" id="hub4"></div>
                        <div class="camera" id="camera4">
                            <div class="camera-lens" id="lens4"></div>
                        </div>
                        <div class="image-output" id="output4"></div>
                        <div class="pov-effect" id="pov4"></div>
                        <div class="hologram-image-container" id="holoImg4">
                            <img src="man.png" alt="man applications"
                                onerror="this.src='https://placehold.co/400x480/2c1e4a/AA99FF?text=man.png'">
                            <span class="image-caption">man.png</span>
                        </div>
                    </div>
                </div>
                <div class="content-column">
                    <div class="slide-title">Applications</div>
                    <div class="uses-grid">
                        <div class="use-card">
                            <h4>🏥 Medical</h4>Real-time 3D imaging for remote surgery
                        </div>
                        <div class="use-card">
                            <h4>🏛️ Education</h4>Floating artifact holograms in museums
                        </div>
                        <div class="use-card">
                            <h4>📐 Entertainment</h4>Volumetric prototyping and design reviews
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        (function () {
            // inject 6 blades into each visual container
            const visualIds = ['visual1', 'visual2', 'visual3', 'visual4'];
            visualIds.forEach(id => {
                const vis = document.getElementById(id);
                if (vis) {
                    vis.querySelectorAll('.fan-blade').forEach(b => b.remove());
                    for (let i = 0; i < 6; i++) {
                        const blade = document.createElement('div');
                        blade.className = 'fan-blade';
                        blade.style.transform = `translateX(-50%) rotate(${i * 60}deg)`;
                        vis.appendChild(blade);
                    }
                }
            });

            // slide navigation
            const slidesWrapper = document.getElementById('slidesWrapper');
            const prevBtn = document.getElementById('prevSlide');
            const nextBtn = document.getElementById('nextSlide');
            const dots = document.querySelectorAll('.dot');
            const slides = document.querySelectorAll('.slide');
            let currentSlide = 0;

            function updateDots(index) {
                dots.forEach((dot, i) => dot.classList.toggle('active', i === index));
            }
            function goToSlide(index) {
                if (index >= 0 && index < slides.length) {
                    slidesWrapper.scrollTo({ left: slides[index].offsetLeft, behavior: 'smooth' });
                    currentSlide = index;
                    updateDots(index);
                }
            }
            prevBtn.addEventListener('click', () => goToSlide(currentSlide - 1));
            nextBtn.addEventListener('click', () => goToSlide(currentSlide + 1));
            slidesWrapper.addEventListener('scroll', () => {
                if (!slides.length) return;
                const idx = Math.round(slidesWrapper.scrollLeft / slides[0].offsetWidth);
                if (idx >= 0 && idx < slides.length && idx !== currentSlide) {
                    currentSlide = idx; updateDots(idx);
                }
            });

            // ----- AUTO-RUN SEQUENCE for slide3 (glow + man.png appears on step5) -----
            const stepRows = {
                0: document.getElementById('step1Row'),
                1: document.getElementById('step2Row'),
                2: document.getElementById('step3Row'),
                3: document.getElementById('step4Row'),
                4: document.getElementById('step5Row')
            };
            const motor3 = document.getElementById('motor3');
            const hub3 = document.getElementById('hub3');
            const camera3 = document.getElementById('camera3');
            const lens3 = document.getElementById('lens3');
            const output3 = document.getElementById('output3');
            const pov3 = document.getElementById('pov3');
            const holoImg3 = document.getElementById('holoImg3'); // container with man.png
            const blades3 = document.querySelectorAll('#visual3 .fan-blade');

            function removeGlow() {
                if (motor3) motor3.classList.remove('glow-motor');
                if (hub3) hub3.classList.remove('glow-hub');
                if (camera3) camera3.classList.remove('glow-camera');
                if (lens3) lens3.classList.remove('glow-lens');
                blades3.forEach(b => b.classList.remove('glow-blade', 'rotating'));
                if (output3) output3.style.opacity = '0';
                if (pov3) pov3.style.opacity = '0';
                if (holoImg3) holoImg3.style.opacity = '0';   // hide image container
            }

            function applyStep(step) {
                removeGlow();
                for (let i = 0; i < 5; i++) {
                    if (stepRows[i]) stepRows[i].classList.toggle('active-step', i === step);
                }

                switch (step) {
                    case 0: if (motor3) motor3.classList.add('glow-motor'); break;
                    case 1: if (camera3) camera3.classList.add('glow-camera'); if (lens3) lens3.classList.add('glow-lens'); break;
                    case 2: if (camera3) camera3.classList.add('glow-camera'); if (lens3) lens3.classList.add('glow-lens'); break;
                    case 3: if (hub3) hub3.classList.add('glow-hub'); blades3.forEach(b => b.classList.add('glow-blade')); break;
                    case 4:
                        blades3.forEach(b => b.classList.add('glow-blade', 'rotating'));
                        if (output3) output3.style.opacity = '0.3';
                        if (pov3) pov3.style.opacity = '0.5';
                        if (holoImg3) {
                            holoImg3.style.opacity = '1';   // man.png appears with no border/background
                            // add a soft white glow filter to the image (optional)
                            const img = holoImg3.querySelector('img');
                            if (img) img.style.filter = 'drop-shadow(0 0 18px #FFFFFF)';
                        }
                        break;
                }
            }

            // reset image filter when not in step 4 (optional, but we remove in removeGlow? we can manage)
            // extend removeGlow to also reset image filter
            const originalRemove = removeGlow;
            removeGlow = function () {
                originalRemove();
                if (holoImg3) {
                    const img = holoImg3.querySelector('img');
                    if (img) img.style.filter = 'drop-shadow(0 0 8px rgba(255,255,255,0.3))'; // subtle default
                }
            };

            let currentStep = 0;
            setInterval(() => {
                currentStep = (currentStep + 1) % 5;
                applyStep(currentStep);
            }, 3500);
            applyStep(0); // start
        })();
    </script>
</body>

</html>
