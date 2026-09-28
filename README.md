
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MHD-SAMS-SROS | Painel Tático da Armadura Espacial</title>
    <!-- Estilos CSS3 Integrados -->
    <style>
        :root {
            --bg-dark: #07090e;
            --panel-bg: rgba(14, 20, 30, 0.85);
            --border-color: rgba(0, 240, 255, 0.25);
            --border-highlight: #00f0ff;
            --text-main: #d1e4f0;
            --text-muted: #627d98;
            --cyan-glow: #00f0ff;
            --cyan-dim: #0088aa;
            --dark-metal: #121820;
            --titanium: #8a99ad;
            --alert-red: #ff3366;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: var(--bg-dark);
            color: var(--text-main);
            font-size: 13px;
        }

        /* Moldura estilo HUD Vingadores */
        .hud-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            box-shadow: inset 0 0 80px rgba(0, 240, 255, 0.1);
            z-index: 10;
        }

        .hud-grid {
            position: absolute;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(rgba(0, 240, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 240, 255, 0.03) 1px, transparent 1px);
            background-size: 40px 40px;
            pointer-events: none;
        }

        /* Container Principal */
        #app-container {
            display: grid;
            grid-template-columns: 320px 1fr 340px;
            grid-template-rows: 60px 1fr 180px;
            width: 100vw;
            height: 100vh;
            position: relative;
            z-index: 20;
        }

        /* Cabeçalho */
        header {
            grid-column: 1 / -1;
            background: rgba(10, 13, 18, 0.9);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 20px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.5);
        }

        .brand-title {
            font-size: 16px;
            font-weight: 800;
            letter-spacing: 2px;
            color: var(--cyan-glow);
            text-shadow: 0 0 10px rgba(0, 240, 255, 0.5);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .brand-title span {
            color: var(--titanium);
            font-size: 11px;
            padding: 2px 6px;
            background: rgba(255,255,255,0.05);
            border-radius: 3px;
            border: 1px solid var(--border-color);
        }

        .status-badge {
            display: flex;
            gap: 20px;
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .status-item {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .dot {
            width: 8px;
            height: 8px;
            background-color: var(--cyan-glow);
            border-radius: 50%;
            box-shadow: 0 0 8px var(--cyan-glow);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.4; transform: scale(0.8); }
        }

        /* Painéis Laterais e Inferiores */
        .panel {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            backdrop-filter: blur(10px);
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow-y: auto;
            margin: 10px;
            border-radius: 4px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .panel-title {
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: var(--cyan-glow);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 6px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Canvas 3D Central */
        #viewport-3d {
            grid-column: 2;
            grid-row: 2;
            position: relative;
            width: 100%;
            height: 100%;
            cursor: grab;
        }

        #viewport-3d:active {
            cursor: grabbing;
        }

        /* Controls e Inputs */
        .control-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .control-label {
            display: flex;
            justify-content: space-between;
            font-size: 11px;
            color: var(--text-muted);
        }

        .control-label span.val {
            color: var(--cyan-glow);
            font-family: monospace;
            font-weight: bold;
        }

        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: rgba(0, 240, 255, 0.1);
            height: 4px;
            border-radius: 2px;
            outline: none;
        }

        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background: var(--cyan-glow);
            cursor: pointer;
            box-shadow: 0 0 10px var(--cyan-glow);
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
        }

        .btn-tactic {
            background: rgba(0, 240, 255, 0.05);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 8px;
            font-size: 11px;
            border-radius: 3px;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-tactic:hover, .btn-tactic.active {
            background: rgba(0, 240, 255, 0.2);
            border-color: var(--cyan-glow);
            color: #fff;
            box-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
        }

        /* Botão Tático de Simulação de Impacto */
        .btn-impact {
            background: rgba(0, 240, 255, 0.15);
            border: 1px solid var(--cyan-glow);
            color: var(--cyan-glow);
            padding: 10px;
            font-size: 11px;
            font-weight: bold;
            border-radius: 3px;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.2);
        }

        .btn-impact:hover {
            background: var(--cyan-glow);
            color: #000;
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.6);
        }

        /* Bloco de Equações em Destaque */
        .eq-card {
            background: rgba(0, 0, 0, 0.4);
            border-left: 3px solid var(--cyan-glow);
            padding: 8px;
            font-family: 'Courier New', Courier, monospace;
            font-size: 11px;
            color: var(--cyan-glow);
            border-radius: 0 4px 4px 0;
            line-height: 1.4;
        }

        /* Rodapé de Telemetria */
        footer {
            grid-column: 1 / -1;
            grid-row: 3;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            padding: 0 10px 10px 10px;
        }

        .metric-card {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            padding: 10px;
            border-radius: 4px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .metric-value {
            font-size: 20px;
            font-weight: 800;
            font-family: monospace;
            color: var(--cyan-glow);
            text-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
        }

        /* Canvas de Gráficos em Tempo Real */
        canvas.chart {
            width: 100%;
            height: 50px;
            background: rgba(0,0,0,0.3);
            border-radius: 2px;
            margin-top: 5px;
        }

        /* Scrollbar customizada */
        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.2);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--cyan-dim);
            border-radius: 2px;
        }
    </style>

    <!-- Importação do Three.js para Renderização 3D autônoma -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <!-- OrbitControls para navegação com o mouse -->
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
</head>
<body>

    <div class="hud-grid"></div>
    <div class="hud-overlay"></div>

    <div id="app-container">
        <!-- BARRA SUPERIOR DE STATUS -->
        <header>
            <div class="brand-title">
                MHD-SAMS-SROS <span>v4.2 ULTRA</span>
            </div>
            <div class="status-badge">
                <div class="status-item"><div class="dot"></div> REATOR BETAVOLTAICO: ATIVO</div>
                <div class="status-item"><div class="dot"></div> CAMPO MHD: 4.2 T</div>
                <div class="status-item"><div class="dot"></div> CRIO-NÚCLEO: 4.2 K</div>
            </div>
        </header>

        <!-- PAINEL ESQUERDO: CONTROLE DOS SISTEMAS -->
        <div class="panel" style="grid-column: 1; grid-row: 2;">
            <div class="panel-title">SUBSISTEMAS TÁTICOS</div>
            
            <div class="control-group">
                <div class="control-label">MHD (Campo Magnético) <span class="val" id="val-mhd">4.2 T</span></div>
                <input type="range" id="input-mhd" min="0" max="10" step="0.1" value="4.2">
            </div>

            <div class="control-group">
                <div class="control-label">SROS (Taxa de CO₂) <span class="val" id="val-co2">0.015 mol/m</span></div>
                <input type="range" id="input-co2" min="0.005" max="0.05" step="0.001" value="0.015">
            </div>

            <div class="control-group">
                <div class="control-label">SAMS (Tensão Reológica) <span class="val" id="val-sams">1.2 ms</span></div>
                <input type="range" id="input-sams" min="0.1" max="5.0" step="0.1" value="1.2">
            </div>

            <div class="panel-title" style="margin-top: 10px;">TESTE DE BALÍSTICA</div>
            <button class="btn-impact" onclick="simulateImpact()">💥 SIMULAR IMPACTO CINÉTICO</button>

            <div class="panel-title" style="margin-top: 10px;">CÂMERA E CAMADAS</div>
            <div class="btn-grid">
                <button class="btn-tactic active" onclick="setCameraView('front')">Frontal</button>
                <button class="btn-tactic" onclick="setCameraView('back')">Posterior</button>
                <button class="btn-tactic" onclick="setCameraView('side')">Lateral</button>
                <button class="btn-tactic" onclick="setCameraView('top')">Superior</button>
            </div>

            <div class="panel-title" style="margin-top: 10px;">MODO DE VISUALIZAÇÃO</div>
            <div class="btn-grid">
                <button class="btn-tactic active" id="btn-layer-ext" onclick="setLayerMode('external')">Blindagem</button>
                <button class="btn-tactic" id="btn-layer-xray" onclick="setLayerMode('xray')">Raio-X (Interno)</button>
            </div>

            <div class="panel-title" style="margin-top: 10px;">EQUAÇÃO REVOLUTA</div>
            <div class="eq-card">
                P_mag = B² / (2μ₀)<br>
                P_mag = <span id="eq-pmag">7.02</span> MPa<br>
                F = q(E + v × B)
            </div>
        </div>

        <!-- VIEWPORT 3D CENTRAL -->
        <div id="viewport-3d"></div>

        <!-- PAINEL DIREITO: MEMORIAL TÉCNICO & FÍSICA -->
        <div class="panel" style="grid-column: 3; grid-row: 2;">
            <div class="panel-title">TELEMETRIA AMBIENTAL</div>
            
            <div class="eq-card">
                <b>Ciclo SROS (CO₂ → C + O₂)</b><br>
                ΔG = ΔH - TΔS (+394.4 kJ/mol)<br>
                Potência Mínima: <span id="eq-power">98.6</span> W
            </div>

            <div class="eq-card">
                <b>Viscosidade SAMS</b><br>
                τ = τ₀ + η(E) × (dγ/dt)ⁿ<br>
                Dissipação Cinética: ΔE = ½m(vᵢ² - v_f²)
            </div>

            <div class="panel-title">ESTADO DO IMPACTO</div>
            <div style="font-size: 11px; display: flex; flex-direction: column; gap: 6px;">
                <div>Projétil Cinético: <b style="color:var(--cyan-glow)" id="impact-status">AGUARDANDO</b></div>
                <div>Energia Absorvida: <b style="color:var(--cyan-glow)" id="impact-energy">0.00 kJ</b></div>
                <div>Tempo Resposta SAMS: <b style="color:var(--cyan-glow)" id="impact-time">0.0 ms</b></div>
                <div>Eficácia do Escudo: <b style="color:var(--cyan-glow)" id="shield-eff">100%</b></div>
            </div>

            <div class="panel-title">PARADOXO TÉRMICO (C.A.R.S.)</div>
            <div style="font-size: 11px; color: var(--text-muted);">
                A dissipação térmica e radiativa do plasma é bombeada ativamente para as placas radiadoras de grafeno nas asas traseiras por fluido magnetocalórico.
            </div>
        </div>

        <!-- RODAPÉ DE MONITORES DE GRÁFICOS EM TEMPO REAL -->
        <footer>
            <div class="metric-card">
                <div class="control-label">PRESSÃO MAGNÉTICA (P_mag)</div>
                <div class="metric-value" id="card-pmag">7.02 MPa</div>
                <canvas id="chart-pmag" class="chart"></canvas>
            </div>
            <div class="metric-card">
                <div class="control-label">POTÊNCIA SROS</div>
                <div class="metric-value" id="card-power">98.6 W</div>
                <canvas id="chart-sros" class="chart"></canvas>
            </div>
            <div class="metric-card">
                <div class="control-label">RESPOSTA SAMS</div>
                <div class="metric-value" id="card-sams">1.2 ms</div>
                <canvas id="chart-sams" class="chart"></canvas>
            </div>
            <div class="metric-card">
                <div class="control-label">ESTABILIDADE DO NÚCLEO C-14</div>
                <div class="metric-value" style="color:var(--cyan-glow)">99.99 %</div>
                <canvas id="chart-core" class="chart"></canvas>
            </div>
        </footer>
    </div>

    <!-- SCRIPT TÁTICO JAVASCRIPT3 -->
    <script>
        // --- 1. CONFIGURAÇÃO DA CENA 3D (THREE.JS) ---
        const container = document.getElementById('viewport-3d');
        const scene = new THREE.Scene();
        scene.fog = new THREE.FogExp2(0x07090e, 0.15);

        const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
        camera.position.set(0, 0, 5.5);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(container.clientWidth, container.clientHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        container.appendChild(renderer.domElement);

        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;

        // --- ILUMINAÇÃO DE ESTILO CINEMATOGRÁFICO ---
        const ambientLight = new THREE.AmbientLight(0x0a1420, 1.5);
        scene.add(ambientLight);

        const cyanLight = new THREE.PointLight(0x00f0ff, 2, 10);
        cyanLight.position.set(2, 3, 2);
        scene.add(cyanLight);

        const backLight = new THREE.DirectionalLight(0x8a99ad, 1);
        backLight.position.set(-2, -2, -2);
        scene.add(backLight);

        // --- GEOMETRIA DA ARMADURA ESPACIAL ---
        const suitGroup = new THREE.Group();

        // Materiais
        const darkArmorMaterial = new THREE.MeshStandardMaterial({
            color: 0x121820,
            roughness: 0.3,
            metalness: 0.85
        });

        const titaniumMaterial = new THREE.MeshStandardMaterial({
            color: 0x8a99ad,
            roughness: 0.2,
            metalness: 0.95
        });

        const cyanGlowMaterial = new THREE.MeshBasicMaterial({
            color: 0x00f0ff,
            wireframe: false
        });

        // Torso / Reator Arc Central
        const torsoGeo = new THREE.CylinderGeometry(0.5, 0.35, 1.2, 8);
        const torso = new THREE.Mesh(torsoGeo, darkArmorMaterial);
        suitGroup.add(torso);

        // Núcleo Betavoltaico Arc Reator (Peitoral)
        const coreGeo = new THREE.TorusGeometry(0.12, 0.03, 16, 32);
        const core = new THREE.Mesh(coreGeo, cyanGlowMaterial);
        core.position.set(0, 0.25, 0.4);
        core.rotation.x = Math.PI / 2;
        suitGroup.add(core);

        // Capacete
        const helmetGeo = new THREE.DodecahedronGeometry(0.32, 1);
        const helmet = new THREE.Mesh(helmetGeo, darkArmorMaterial);
        helmet.position.set(0, 0.85, 0);
        helmet.scale.set(1, 1.1, 1);
        suitGroup.add(helmet);

        // Viseira Ciano Tática
        const visorGeo = new THREE.BoxGeometry(0.35, 0.08, 0.25);
        const visor = new THREE.Mesh(visorGeo, cyanGlowMaterial);
        visor.position.set(0, 0.88, 0.22);
        suitGroup.add(visor);

        // Ombreiras (Titanium)
        const shoulderGeo = new THREE.BoxGeometry(0.35, 0.25, 0.35);
        const leftShoulder = new THREE.Mesh(shoulderGeo, titaniumMaterial);
        leftShoulder.position.set(-0.6, 0.5, 0);
        const rightShoulder = leftShoulder.clone();
        rightShoulder.position.x = 0.6;
        suitGroup.add(leftShoulder);
        suitGroup.add(rightShoulder);

        // Escudo Magnético MHD (Efeito de Campo de Força)
        const shieldGeo = new THREE.IcosahedronGeometry(1.6, 2);
        const shieldMaterial = new THREE.MeshBasicMaterial({
            color: 0x00f0ff,
            wireframe: true,
            transparent: true,
            opacity: 0.15
        });
        const shieldMesh = new THREE.Mesh(shieldGeo, shieldMaterial);
        suitGroup.add(shieldMesh);

        // --- MÓDULO DE PROJÉTIL E IMPACTO BALÍSTICO ---
        const projectileGeo = new THREE.SphereGeometry(0.06, 16, 16);
        const projectileMat = new THREE.MeshBasicMaterial({ color: 0xff3366 });
        const projectileMesh = new THREE.Mesh(projectileGeo, projectileMat);
        projectileMesh.visible = false;
        scene.add(projectileMesh);

        // Onda de Choque de Impacto
        const shockwaveGeo = new THREE.RingGeometry(0.05, 0.4, 32);
        const shockwaveMat = new THREE.MeshBasicMaterial({ color: 0x00f0ff, side: THREE.DoubleSide, transparent: true, opacity: 0 });
        const shockwaveMesh = new THREE.Mesh(shockwaveGeo, shockwaveMat);
        shockwaveMesh.visible = false;
        scene.add(shockwaveMesh);

        scene.add(suitGroup);

        // --- 2. LÓGICA DE ATUALIZAÇÃO DA FÍSICA E INTERFACE ---
        const inputMhd = document.getElementById('input-mhd');
        const inputCo2 = document.getElementById('input-co2');
        const inputSams = document.getElementById('input-sams');

        function updatePhysics() {
            const B = parseFloat(inputMhd.value);
            const n_co2 = parseFloat(inputCo2.value);
            const sams = parseFloat(inputSams.value);

            // Equações
            const mu0 = 4 * Math.PI * 1e-7;
            const P_mag = (Math.pow(B, 2) / (2 * mu0)) / 1e6; // MPa
            const P_sros = n_co2 * 394.4 * 1000 / 60; // Watts

            // Atualização do DOM
            document.getElementById('val-mhd').innerText = B.toFixed(1) + ' T';
            document.getElementById('val-co2').innerText = n_co2.toFixed(3) + ' mol/m';
            document.getElementById('val-sams').innerText = sams.toFixed(1) + ' ms';

            document.getElementById('eq-pmag').innerText = P_mag.toFixed(2);
            document.getElementById('eq-power').innerText = P_sros.toFixed(1);

            document.getElementById('card-pmag').innerText = P_mag.toFixed(2) + ' MPa';
            document.getElementById('card-power').innerText = P_sros.toFixed(1) + ' W';
            document.getElementById('card-sams').innerText = sams.toFixed(1) + ' ms';

            // Ajuste do efeito visual 3D do escudo MHD
            shieldMesh.scale.setScalar(1 + (B / 20));
            shieldMaterial.opacity = 0.05 + (B / 25);

            // Eficiência do escudo
            const eff = Math.min(100, (B / 4.2) * 100);
            document.getElementById('shield-eff').innerText = eff.toFixed(0) + '%';
        }

        inputMhd.addEventListener('input', updatePhysics);
        inputCo2.addEventListener('input', updatePhysics);
        inputSams.addEventListener('input', updatePhysics);

        // --- SIMULAÇÃO DE IMPACTO CINÉTICO BALÍSTICO ---
        let isSimulatingImpact = false;
        window.simulateImpact = function() {
            if(isSimulatingImpact) return;
            isSimulatingImpact = true;

            const startPos = new THREE.Vector3(5, 1, 3);
            const targetPos = new THREE.Vector3(0, 0.25, 0.4); // Peitoral
            projectileMesh.position.copy(startPos);
            projectileMesh.visible = true;

            document.getElementById('impact-status').innerText = 'INTERCEPTANDO...';
            document.getElementById('impact-status').style.color = '#ff3366';

            let progress = 0;
            const startTime = performance.now();

            function animateProjectile() {
                progress += 0.04;
                projectileMesh.position.lerpVectors(startPos, targetPos, progress);

                if(progress < 1) {
                    requestAnimationFrame(animateProjectile);
                } else {
                    // Colisão no Escudo Magnético MHD + Fluido SAMS
                    projectileMesh.visible = false;
                    
                    // Dispara onda de choque
                    shockwaveMesh.position.copy(targetPos);
                    shockwaveMesh.lookAt(camera.position);
                    shockwaveMesh.visible = true;
                    shockwaveMat.opacity = 1;
                    shockwaveMesh.scale.set(1, 1, 1);

                    // Deforma temporariamente o escudo MHD
                    shieldMaterial.color.setHex(0xff3366);
                    shieldMaterial.opacity = 0.8;

                    const B = parseFloat(inputMhd.value);
                    const samsTime = parseFloat(inputSams.value);
                    const energy = (0.5 * 0.05 * Math.pow(1200, 2)) / 1000; // 36 kJ de impacto cinético

                    document.getElementById('impact-status').innerText = 'DEFLETIDO (MHD-SAMS)';
                    document.getElementById('impact-status').style.color = '#00f0ff';
                    document.getElementById('impact-energy').innerText = energy.toFixed(2) + ' kJ';
                    document.getElementById('impact-time').innerText = samsTime.toFixed(1) + ' ms';

                    // Animação de dissipação da onda de choque
                    let shockProgress = 0;
                    function fadeShock() {
                        shockProgress += 0.05;
                        shockwaveMesh.scale.addScalar(0.2);
                        shockwaveMat.opacity = 1 - shockProgress;

                        if(shockProgress < 1) {
                            requestAnimationFrame(fadeShock);
                        } else {
                            shockwaveMesh.visible = false;
                            shieldMaterial.color.setHex(0x00f0ff);
                            updatePhysics();
                            isSimulatingImpact = false;
                        }
                    }
                    fadeShock();
                }
            }
            animateProjectile();
        };

        // --- 3. CONTROLE DE CÂMERA E MODO DE RAIO-X ---
        window.setCameraView = function(view) {
            document.querySelectorAll('.btn-grid button').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');

            switch(view) {
                case 'front': camera.position.set(0, 0, 5.5); break;
                case 'back': camera.position.set(0, 0, -5.5); break;
                case 'side': camera.position.set(5.5, 0, 0); break;
                case 'top': camera.position.set(0, 5.5, 0.1); break;
            }
            controls.update();
        };

        window.setLayerMode = function(mode) {
            document.getElementById('btn-layer-ext').classList.remove('active');
            document.getElementById('btn-layer-xray').classList.remove('active');

            if(mode === 'xray') {
                document.getElementById('btn-layer-xray').classList.add('active');
                darkArmorMaterial.wireframe = true;
                titaniumMaterial.wireframe = true;
            } else {
                document.getElementById('btn-layer-ext').classList.add('active');
                darkArmorMaterial.wireframe = false;
                titaniumMaterial.wireframe = false;
            }
        };

        // --- 4. GRÁFICOS DINÂMICOS EM TEMPO REAL DA TELEMETRIA ---
        function createChart(canvasId, color) {
            const canvas = document.getElementById(canvasId);
            const ctx = canvas.getContext('2d');
            let data = new Array(30).fill(0);

            function draw() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                ctx.beginPath();
                ctx.strokeStyle = color;
                ctx.lineWidth = 1.5;

                const step = canvas.width / (data.length - 1);
                for(let i = 0; i < data.length; i++) {
                    const x = i * step;
                    const y = canvas.height - (data[i] * canvas.height);
                    if(i === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
                ctx.stroke();
            }

            return {
                update: (val) => {
                    data.shift();
                    data.push(val);
                    draw();
                }
            };
        }

        const chartPmag = createChart('chart-pmag', '#00f0ff');
        const chartSros = createChart('chart-sros', '#00f0ff');
        const chartSams = createChart('chart-sams', '#0088aa');
        const chartCore = createChart('chart-core', '#00f0ff');

        // --- LOOP DE ANIMAÇÃO 60 FPS ---
        function animate() {
            requestAnimationFrame(animate);

            // Rotação sutil da armadura
            suitGroup.rotation.y += 0.003;
            shieldMesh.rotation.x += 0.002;
            core.rotation.z += 0.02;

            controls.update();
            renderer.render(scene, camera);

            // Atualização dos gráficos dinâmicos
            const B = parseFloat(inputMhd.value);
            chartPmag.update((B / 10) + (Math.random() * 0.05));
            chartSros.update(0.5 + (Math.random() * 0.1));
            chartSams.update(0.4 + (Math.random() * 0.08));
            chartCore.update(0.95 + (Math.random() * 0.03));
        }

        // Redimensionamento de janela
        window.addEventListener('resize', () => {
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        });

        // Inicialização
        updatePhysics();
        animate();
    </script>
</body>
</html>
