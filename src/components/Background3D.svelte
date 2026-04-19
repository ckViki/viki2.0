<script>
    // @ts-nocheck
    import { onMount, onDestroy } from 'svelte';
    import { browser } from '$app/environment';

    let container;
    let scene, camera, renderer;
    let frameId;
    let THREE;
    let startTime = 0;
    let _tmpV;       // initialized in init()
    let _tmpV2;      // initialized in init()
    let _up;         // initialized in init()
    let solarPivot;  // group holding entire solar system
    let moonData;    // { pivot } — moon orbiting Earth

    let starField;
    let planetGroups  = [];
    let rocketObjects = [];
    let satellites    = [];

    /* ━━━ PLANET CONFIGS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */
    const PLANET_CONFIGS = [
        { name: 'mercury', radius: 0.55, distance: 7,   speed: 0.90, tilt: 0.10, hasRing: false },
        { name: 'venus',   radius: 0.80, distance: 11,  speed: 0.60, tilt: 0.05, hasRing: false },
        { name: 'earth',   radius: 0.90, distance: 15,  speed: 0.42, tilt: 0.40, hasRing: false },
        { name: 'mars',    radius: 0.60, distance: 19,  speed: 0.30, tilt: 0.45, hasRing: false },
        { name: 'jupiter', radius: 1.80, distance: 25,  speed: 0.18, tilt: 0.05, hasRing: false },
        { name: 'saturn',  radius: 1.40, distance: 31,  speed: 0.12, tilt: 0.45, hasRing: true  },
        { name: 'uranus',  radius: 1.00, distance: 37,  speed: 0.08, tilt: 1.47, hasRing: true  },
    ];

    /* ━━━ ROCKET CONFIGS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */
    // type:'solar' → orbits whole system at large R
    // type:'earth' → elliptical orbit near Earth (going & coming)
    const ROCKET_CONFIGS = [
        { type: 'solar', orbitR: 46, speed: 0.24, phase: 0.0,  inc:  0.10 }, // big sweep
        { type: 'earth', orbitR: 2.8, speed: 1.8,  phase: 0.5,  inc:  0.25 }, // near Earth
        { type: 'earth', orbitR: 3.5, speed: 1.4,  phase: 3.2,  inc: -0.20 }, // near Earth
        { type: 'solar', orbitR: 22, speed: 0.40, phase: 1.8,  inc:  0.15 }, // inner sweep
    ];

    /* ━━━ SATELLITE CONFIGS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */
    // type:'planet' → orbits a specific planet  |  type:'solar' → orbits solar center
    const SATELLITE_CONFIGS = [
        { type: 'planet', planetIdx: 2, orbitR: 1.9, speed: 3.0, phase: 0.0,  inc:  0.20 }, // Earth
        { type: 'planet', planetIdx: 2, orbitR: 2.5, speed: 2.2, phase: 1.8,  inc: -0.40 }, // Earth
        { type: 'planet', planetIdx: 3, orbitR: 2.0, speed: 2.5, phase: 3.5,  inc:  0.30 }, // Mars
        { type: 'planet', planetIdx: 4, orbitR: 1.3, speed: 1.8, phase: 1.2,  inc:  0.15 }, // Jupiter
        { type: 'solar',  orbitR: 20,  speed: 0.30, phase: 2.5,  inc:  0.06 },               // free solar
    ];

    onMount(async () => {
        if (!browser) return;
        THREE = await import('three');
        startTime = performance.now();
        init();
        animate();
        window.addEventListener('resize', onWindowResize);
    });

    onDestroy(() => {
        if (!browser) return;
        if (frameId) cancelAnimationFrame(frameId);
        window.removeEventListener('resize', onWindowResize);
        if (renderer) renderer.dispose();
    });

    /* ━━━ CANVAS TEXTURE GENERATION ━━━━━━━━━━━━━━━━━━━━━━━━━━━ */

    function makeTex(drawFn, w = 512, h = 256) {
        const cvs = document.createElement('canvas');
        cvs.width = w; cvs.height = h;
        drawFn(cvs.getContext('2d'), w, h);
        return new THREE.CanvasTexture(cvs);
    }

    const TEXTURES = {
        mercury: (ctx, w, h) => {
            ctx.fillStyle = '#8a8888'; ctx.fillRect(0, 0, w, h);
            for (let i = 0; i < 55; i++) {
                const x = Math.random()*w, y = Math.random()*h, r = 2 + Math.random()*14;
                ctx.beginPath(); ctx.arc(x, y, r, 0, Math.PI*2);
                ctx.fillStyle = `rgba(55,52,52,${0.25 + Math.random()*0.45})`; ctx.fill();
                ctx.strokeStyle = `rgba(160,155,155,0.4)`; ctx.lineWidth = 0.8; ctx.stroke();
            }
            // Slight radial variation
            const g = ctx.createRadialGradient(w*0.35, h*0.4, 0, w*0.5, h*0.5, w*0.5);
            g.addColorStop(0,'rgba(200,195,190,0.18)'); g.addColorStop(1,'rgba(30,28,28,0.18)');
            ctx.fillStyle = g; ctx.fillRect(0,0,w,h);
        },
        venus: (ctx, w, h) => {
            const g = ctx.createLinearGradient(0,0,0,h);
            g.addColorStop(0,'#d4914a'); g.addColorStop(0.25,'#f0b865'); g.addColorStop(0.5,'#f5cc70');
            g.addColorStop(0.75,'#dd9940'); g.addColorStop(1,'#c88038');
            ctx.fillStyle = g; ctx.fillRect(0,0,w,h);
            for (let i = 0; i < 18; i++) {
                ctx.beginPath();
                ctx.ellipse(Math.random()*w, Math.random()*h, 50+Math.random()*90, 7+Math.random()*14, Math.random()*Math.PI, 0, Math.PI*2);
                ctx.fillStyle = `rgba(255,215,100,${0.07+Math.random()*0.13})`; ctx.fill();
            }
        },
        earth: (ctx, w, h) => {
            ctx.fillStyle = '#1b5fa5'; ctx.fillRect(0,0,w,h);
            const continents = [
                { x:0.18, y:0.35, rx:0.07, ry:0.18, r:-0.3, c:'#2d7d20' },
                { x:0.53, y:0.30, rx:0.20, ry:0.15, r:0.1,  c:'#3a8830' },
                { x:0.52, y:0.58, rx:0.055,ry:0.15, r:0,    c:'#8a7028' },
                { x:0.27, y:0.63, rx:0.045,ry:0.14, r:0.2,  c:'#3a7820' },
                { x:0.80, y:0.63, rx:0.065,ry:0.075,r:0,    c:'#aa8830' },
                { x:0.90, y:0.40, rx:0.025,ry:0.06, r:0,    c:'#2d7820' },
            ];
            continents.forEach(c => {
                ctx.fillStyle = c.c;
                ctx.beginPath(); ctx.ellipse(c.x*w, c.y*h, c.rx*w, c.ry*h, c.r, 0, Math.PI*2); ctx.fill();
            });
            ctx.fillStyle = 'rgba(245,248,255,0.78)';
            ctx.fillRect(0,0,w,h*0.065); ctx.fillRect(0,h*0.935,w,h*0.065);
            for (let i = 0; i < 30; i++) {
                ctx.beginPath();
                ctx.ellipse(Math.random()*w, Math.random()*h, 20+Math.random()*60, 6+Math.random()*12, Math.random()*Math.PI, 0, Math.PI*2);
                ctx.fillStyle = `rgba(255,255,255,${0.07+Math.random()*0.16})`; ctx.fill();
            }
        },
        mars: (ctx, w, h) => {
            const g = ctx.createLinearGradient(0,0,0,h);
            g.addColorStop(0,'#c23310'); g.addColorStop(0.5,'#e0441e'); g.addColorStop(1,'#b42200');
            ctx.fillStyle = g; ctx.fillRect(0,0,w,h);
            for (let i = 0; i < 18; i++) {
                ctx.beginPath();
                ctx.ellipse(Math.random()*w, Math.random()*h, 15+Math.random()*65, 8+Math.random()*28, Math.random()*Math.PI, 0, Math.PI*2);
                ctx.fillStyle = `rgba(70,15,0,${0.18+Math.random()*0.28})`; ctx.fill();
            }
            ctx.fillStyle = 'rgba(255,235,220,0.60)';
            ctx.fillRect(0,0,w,h*0.065); ctx.fillRect(0,h*0.935,w,h*0.065);
        },
        jupiter: (ctx, w, h) => {
            const bands = ['#c88b3a','#eeab58','#d4943a','#f2b058','#c07032',
                           '#e09040','#d48848','#c07032','#dfa044','#c88b3a'];
            bands.forEach((c,i) => { ctx.fillStyle = c; ctx.fillRect(0, i*(h/bands.length), w, h/bands.length+1); });
            // Great Red Spot
            ctx.fillStyle = 'rgba(185,52,18,0.75)';
            ctx.beginPath(); ctx.ellipse(w*0.64, h*0.55, 38, 20, 0, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = 'rgba(220,90,40,0.35)';
            ctx.beginPath(); ctx.ellipse(w*0.64, h*0.55, 30, 14, 0, 0, Math.PI*2); ctx.fill();
            // Horizontal band details
            for (let i = 0; i < 35; i++) {
                const y = Math.random()*h;
                ctx.strokeStyle = `rgba(${Math.random()>0.5?'255,180,80':'120,65,10'},${0.08+Math.random()*0.13})`;
                ctx.lineWidth = 0.5 + Math.random()*3;
                ctx.beginPath(); ctx.moveTo(0,y);
                ctx.bezierCurveTo(w*0.25,y+(Math.random()-0.5)*10, w*0.75,y+(Math.random()-0.5)*10, w,y); ctx.stroke();
            }
        },
        saturn: (ctx, w, h) => {
            const bands = ['#e8d292','#d4b872','#eedf90','#c8a868','#e0cc80','#d2ae60'];
            bands.forEach((c,i) => { ctx.fillStyle = c; ctx.fillRect(0, i*(h/bands.length), w, h/bands.length+1); });
            for (let i = 0; i < 22; i++) {
                ctx.strokeStyle = `rgba(180,140,50,${0.08+Math.random()*0.10})`; ctx.lineWidth = 0.5+Math.random()*2;
                const y = Math.random()*h;
                ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(w, y+(Math.random()-0.5)*4); ctx.stroke();
            }
        },
        uranus: (ctx, w, h) => {
            const g = ctx.createRadialGradient(w*0.38, h*0.42, 0, w*0.5, h*0.5, w*0.55);
            g.addColorStop(0,'#95eedd'); g.addColorStop(0.5,'#44ccbc'); g.addColorStop(1,'#1a8870');
            ctx.fillStyle = g; ctx.fillRect(0,0,w,h);
            for (let i = 0; i < 8; i++) {
                ctx.fillStyle = `rgba(${i%2?'80,210,195':'30,160,145'},${0.04+Math.random()*0.07})`;
                ctx.fillRect(0, i*(h/8), w, h/8+1);
            }
        },
    };

    /* ━━━ BUILD FUNCTIONS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */

    function buildStars() {
        const geo = new THREE.BufferGeometry();
        const v = new Float32Array(18000 * 3);
        for (let i = 0; i < v.length; i++) v[i] = (Math.random()-0.5) * 900;
        geo.setAttribute('position', new THREE.Float32BufferAttribute(v, 3));
        return new THREE.Points(geo,
            new THREE.PointsMaterial({ color: 0xffffff, size: 0.28, transparent: true, opacity: 0.65 }));
    }

    function buildOrbitRing(distance) {
        // Ultra-thin line in XZ plane — nearly invisible, won't disturb UI
        const pts = [];
        for (let i = 0; i <= 160; i++) {
            const a = (i / 160) * Math.PI * 2;
            pts.push(new THREE.Vector3(Math.cos(a) * distance, 0, Math.sin(a) * distance));
        }
        const geo = new THREE.BufferGeometry().setFromPoints(pts);
        const mat = new THREE.LineBasicMaterial({
            color: 0x8899bb,      // neutral blue-grey
            transparent: true,
            opacity: 0.10        // very very faint
        });
        return new THREE.Line(geo, mat);
    }

    function buildPlanet(cfg) {
        const pivot = new THREE.Group();
        const meshGroup = new THREE.Group();
        meshGroup.rotation.z = cfg.tilt;
        meshGroup.position.x = cfg.distance;

        // Sphere with canvas texture
        const tex = makeTex(TEXTURES[cfg.name]);
        const sphere = new THREE.Mesh(
            new THREE.SphereGeometry(cfg.radius, 64, 64),
            new THREE.MeshStandardMaterial({
                map: tex,
                roughness: 0.75,
                metalness: 0.05,
                // Slight boost so dark side isn't totally black
                emissiveMap: tex,
                emissive: new THREE.Color(0xffffff),
                emissiveIntensity: 0.12,
            })
        );
        meshGroup.add(sphere);

        // Thin atmosphere shell
        const atmColors = {
            mercury: 0x888898, venus: 0xf5b050, earth: 0x4488ff,
            mars: 0xdd5522, jupiter: 0xd4943a, saturn: 0xeedd80, uranus: 0x44ddcc,
        };
        const atm = new THREE.Mesh(
            new THREE.SphereGeometry(cfg.radius * 1.08, 32, 32),
            new THREE.MeshBasicMaterial({
                color: atmColors[cfg.name] || 0xffffff,
                transparent: true, opacity: 0.10, side: THREE.BackSide
            })
        );
        meshGroup.add(atm);

        // Planetary light — illuminates nearby rockets
        const pLight = new THREE.PointLight(atmColors[cfg.name] || 0xffffff, 1.8, cfg.radius * 14);
        meshGroup.add(pLight);

        // Rings for Saturn / Uranus
        if (cfg.hasRing) {
            const ri = cfg.radius * 1.45, ro = cfg.radius * 2.45;
            const ringGeo = new THREE.RingGeometry(ri, ro, 96);
            // Fix UV so inner=0, outer=1
            const pos = ringGeo.attributes.position, uv = ringGeo.attributes.uv;
            for (let i = 0; i < pos.count; i++) {
                const x = pos.getX(i), z = pos.getZ(i);
                const r = Math.sqrt(x*x + z*z);
                uv.setXY(i, (r - ri) / (ro - ri), 0);
            }
            const ringColor = cfg.name === 'uranus' ? 0x66ddcc : 0xddb860;
            const ring = new THREE.Mesh(ringGeo,
                new THREE.MeshBasicMaterial({ color: ringColor, side: THREE.DoubleSide, transparent: true, opacity: 0.55 }));
            ring.rotation.x = cfg.name === 'uranus' ? Math.PI / 2.0 : -Math.PI / 2.6;
            meshGroup.add(ring);
        }

        pivot.add(meshGroup);
        pivot.rotation.y = Math.random() * Math.PI * 2;
        return { pivot, meshGroup, sphere, cfg };
    }

    function buildRocket() {
        const g = new THREE.Group();

        const whiteMat = new THREE.MeshStandardMaterial({ color: 0xf2f2f8, metalness: 0.88, roughness: 0.10 });
        const darkMat  = new THREE.MeshStandardMaterial({ color: 0x2a2a3a, metalness: 0.95, roughness: 0.08 });

        // ── Stage 1: main fuel tank — tall slender cylinder ──
        const s1 = new THREE.Mesh(new THREE.CylinderGeometry(0.10, 0.10, 1.10, 16), whiteMat);
        s1.position.y = -0.10;
        g.add(s1);

        // ── Stage 2: upper stage — slightly narrower ──
        const s2 = new THREE.Mesh(new THREE.CylinderGeometry(0.09, 0.10, 0.55, 16), whiteMat);
        s2.position.y = 0.825;
        g.add(s2);

        // ── Interstage ring — dark band separating stages ──
        const ring = new THREE.Mesh(new THREE.CylinderGeometry(0.102, 0.102, 0.06, 16), darkMat);
        ring.position.y = 0.52;
        g.add(ring);

        // ── Payload fairing — ogive nose ──
        const fairing = new THREE.Mesh(
            new THREE.ConeGeometry(0.09, 0.42, 16),
            new THREE.MeshStandardMaterial({ color: 0xffffff, metalness: 0.6, roughness: 0.18 })
        );
        fairing.position.y = 1.31;
        g.add(fairing);

        // ── Grid fins — 4 near top of Stage 1 (Falcon 9 style) ──
        for (let i = 0; i < 4; i++) {
            const t = (i / 4) * Math.PI * 2 + Math.PI / 4;
            const gf = new THREE.Mesh(
                new THREE.BoxGeometry(0.02, 0.14, 0.13),
                new THREE.MeshStandardMaterial({ color: 0x888898, metalness: 0.92, roughness: 0.12 })
            );
            gf.position.set(Math.cos(t) * 0.11, 0.28, Math.sin(t) * 0.11);
            gf.rotation.y = -t;
            g.add(gf);
        }

        // ── Landing legs — 4 folded struts at base ──
        for (let i = 0; i < 4; i++) {
            const t = (i / 4) * Math.PI * 2;
            const leg = new THREE.Mesh(
                new THREE.BoxGeometry(0.012, 0.28, 0.012),
                new THREE.MeshStandardMaterial({ color: 0x555566, metalness: 0.88, roughness: 0.2 })
            );
            leg.position.set(Math.cos(t) * 0.105, -0.54, Math.sin(t) * 0.105);
            // Angled outward slightly like folded legs
            leg.rotation.z =  Math.cos(t) * 0.18;
            leg.rotation.x = -Math.sin(t) * 0.18;
            g.add(leg);
        }

        // ── Engine bell — flared nozzle (open bottom) ──
        const bell = new THREE.Mesh(
            new THREE.CylinderGeometry(0.06, 0.105, 0.20, 14, 1, true),
            new THREE.MeshStandardMaterial({ color: 0x555570, metalness: 0.97, roughness: 0.04, side: THREE.DoubleSide })
        );
        bell.position.y = -0.76;
        g.add(bell);

        // ── Exhaust: 3-layer realistic shock-diamond plume ──
        // Layer 1: bright white core (hot combustion exhaust)
        const core = new THREE.Mesh(
            new THREE.ConeGeometry(0.045, 0.55, 12),
            new THREE.MeshBasicMaterial({ color: 0xffffff, transparent: true, opacity: 0.96 })
        );
        core.rotation.x = Math.PI;
        core.position.y = -1.03;
        g.add(core);

        // Layer 2: sapphire-blue mid plume (shock diamonds)
        const mid = new THREE.Mesh(
            new THREE.ConeGeometry(0.10, 0.85, 12),
            new THREE.MeshBasicMaterial({ color: 0x88aaff, transparent: true, opacity: 0.65 })
        );
        mid.rotation.x = Math.PI;
        mid.position.y = -1.22;
        g.add(mid);

        // Layer 3: faint amber outer expansion
        const outer = new THREE.Mesh(
            new THREE.ConeGeometry(0.17, 1.10, 12),
            new THREE.MeshBasicMaterial({ color: 0xffbb44, transparent: true, opacity: 0.28 })
        );
        outer.rotation.x = Math.PI;
        outer.position.y = -1.40;
        g.add(outer);

        // ── Engine glow — blue-white (LOX engine realism) ──
        const egMat = new THREE.MeshStandardMaterial({
            color: 0xbbddff, emissive: 0x6699ff,
            emissiveIntensity: 4.5, transparent: true, opacity: 0.88
        });
        const eg = new THREE.Mesh(new THREE.SphereGeometry(0.075, 10, 10), egMat);
        eg.position.y = -0.80;
        g.add(eg);
        g._egMat = egMat;

        // ── Soft blue point light at engine ──
        const el = new THREE.PointLight(0x6699ff, 2.5, 5.5);
        el.position.y = -0.82;
        g.add(el);
        g._el = el;

        return g;
    }

    /* ━━━ BUILD SATELLITE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */
    function buildSatellite() {
        const g = new THREE.Group();

        // Central body — small metallic box
        g.add(new THREE.Mesh(
            new THREE.BoxGeometry(0.14, 0.10, 0.14),
            new THREE.MeshStandardMaterial({ color: 0xccccdd, metalness: 0.92, roughness: 0.08 })
        ));

        // Solar panels — two dark-blue flat wings
        for (const side of [-1, 1]) {
            const panel = new THREE.Mesh(
                new THREE.BoxGeometry(0.45, 0.015, 0.22),
                new THREE.MeshStandardMaterial({
                    color: 0x223388, emissive: 0x112255, emissiveIntensity: 0.5,
                    metalness: 0.7, roughness: 0.2
                })
            );
            panel.position.x = side * 0.34;
            g.add(panel);
        }

        // Antenna mast
        const mast = new THREE.Mesh(
            new THREE.CylinderGeometry(0.006, 0.006, 0.20, 6),
            new THREE.MeshStandardMaterial({ color: 0xffffff, metalness: 0.8 })
        );
        mast.position.set(0, 0.15, 0);
        g.add(mast);

        // Dish
        const dish = new THREE.Mesh(
            new THREE.CylinderGeometry(0.055, 0.055, 0.008, 12),
            new THREE.MeshStandardMaterial({ color: 0xdddddd, metalness: 0.9, roughness: 0.1 })
        );
        dish.position.set(0, 0.24, 0);
        dish.rotation.x = Math.PI / 4;
        g.add(dish);

        return g;
    }

    function buildNebula(color, cx, cy, cz, spread, count = 800) {
        const geo = new THREE.BufferGeometry();
        const v = new Float32Array(count * 3);
        for (let i = 0; i < count; i++) {
            // Gaussian-ish spread: sample in sphere with falloff
            const theta = Math.random() * Math.PI * 2;
            const phi   = Math.acos(2 * Math.random() - 1);
            const r     = spread * Math.pow(Math.random(), 0.5);
            v[i*3]   = cx + r * Math.sin(phi) * Math.cos(theta);
            v[i*3+1] = cy + r * Math.sin(phi) * Math.sin(theta);
            v[i*3+2] = cz + r * Math.cos(phi);
        }
        geo.setAttribute('position', new THREE.Float32BufferAttribute(v, 3));
        return new THREE.Points(geo,
            new THREE.PointsMaterial({
                color, size: 0.45,
                transparent: true, opacity: 0.22,
                blending: THREE.AdditiveBlending, depthWrite: false
            })
        );
    }

    /* ━━━ INIT ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */

    function init() {
        scene = new THREE.Scene();

        camera = new THREE.PerspectiveCamera(55, window.innerWidth / window.innerHeight, 0.1, 2000);
        // Nice cinematic angle — slight elevation, not top-down
        // Solar system is shifted so sun is at bottom-left; planets spread across lower-center
        camera.position.set(0, 10, 52);
        camera.lookAt(0, -5, 0);

        renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        container.appendChild(renderer.domElement);

        /* ── Global lighting (scene level) ── */
        scene.add(new THREE.AmbientLight(0x334466, 2.5));
        const rimLight = new THREE.DirectionalLight(0x4466aa, 0.9);
        rimLight.position.set(-30, 20, -40);
        scene.add(rimLight);

        /* ── Solar system group ──────────────────────────────────────
           Sun sits at world (-8, -12, 0) — bottom-left of screen.
           Planets at distance 7–37 → world x: -1 to +29, y: -12.
           This keeps the whole system BELOW the main portfolio text
           while still visible as a beautiful background element.
        ─────────────────────────────────────────────────────────── */
        solarPivot = new THREE.Group();
        // Sun at world (-18, -7, 0) — left side of screen
        // Planets at dist 7→37: world x = -11 to +19, spreading across full screen
        solarPivot.position.set(-18, -7, 0);
        scene.add(solarPivot);

        // Sun light inside group (moves with the system)
        const sunLight = new THREE.PointLight(0xffe8c0, 6.0, 150);
        solarPivot.add(sunLight);

        /* ── Sun ── */
        const sunTex = makeTex((ctx, w, h) => {
            const g = ctx.createRadialGradient(w*0.45, h*0.45, 0, w*0.5, h*0.5, w*0.5);
            g.addColorStop(0,'#ffffaa'); g.addColorStop(0.3,'#ffcc44'); g.addColorStop(0.7,'#ff9900'); g.addColorStop(1,'#cc5500');
            ctx.fillStyle = g; ctx.fillRect(0,0,w,h);
            for (let i = 0; i < 40; i++) {
                ctx.beginPath();
                ctx.ellipse(Math.random()*w, Math.random()*h, 8+Math.random()*25, 4+Math.random()*12, Math.random()*Math.PI, 0, Math.PI*2);
                ctx.fillStyle = `rgba(255,200,50,${0.1+Math.random()*0.2})`; ctx.fill();
            }
        });
        const sun = new THREE.Mesh(
            new THREE.SphereGeometry(2.0, 48, 48),
            new THREE.MeshStandardMaterial({
                map: sunTex, emissiveMap: sunTex,
                emissive: new THREE.Color(0xffcc44),
                emissiveIntensity: 2.5, roughness: 1.0, metalness: 0.0
            })
        );
        solarPivot.add(sun);

        // Corona layers — also inside group
        [{ r: 3.0, op: 0.09 }, { r: 4.2, op: 0.04 }].forEach(({ r, op }) => {
            solarPivot.add(new THREE.Mesh(
                new THREE.SphereGeometry(r, 32, 32),
                new THREE.MeshBasicMaterial({ color: 0xff8800, transparent: true, opacity: op, side: THREE.BackSide })
            ));
        });

        /* ── Stars ── */
        starField = buildStars();
        scene.add(starField);

        /* ── Nebula / galaxy clouds ── */
        scene.add(buildNebula(0x9944cc, -30, 15, -60, 28, 1200));
        scene.add(buildNebula(0x2255cc,  55, -10, -70, 22, 900));
        scene.add(buildNebula(0xcc2277, -55, -20, -50, 18, 700));
        scene.add(buildNebula(0x22aacc,  70,  20, -40, 16, 600));
        scene.add(buildNebula(0x8833aa,  10,  40, -80, 35, 1400));

        /* ── Orbit rings + Planets — all inside solarPivot ── */
        PLANET_CONFIGS.forEach(cfg => {
            solarPivot.add(buildOrbitRing(cfg.distance));
            const p = buildPlanet(cfg);
            planetGroups.push(p);
            solarPivot.add(p.pivot);
        });

        /* ── Moon orbiting Earth ── */
        const earthGroup = planetGroups[2]; // Earth is index 2
        const moonPivot  = new THREE.Group();
        const moonMesh   = new THREE.Mesh(
            new THREE.SphereGeometry(0.22, 20, 20),
            new THREE.MeshStandardMaterial({
                color: 0xaaaaaa, emissive: 0x444444, emissiveIntensity: 0.3,
                roughness: 0.95, metalness: 0.0
            })
        );
        moonMesh.position.x = 1.65; // distance from Earth centre
        moonPivot.add(moonMesh);
        earthGroup.meshGroup.add(moonPivot);  // moon moves with Earth's world orbit
        moonData = { pivot: moonPivot };

        /* ── Satellites ── */
        SATELLITE_CONFIGS.forEach(scfg => {
            const mesh = buildSatellite();
            mesh.scale.setScalar(0.55);
            mesh._scfg  = scfg;
            mesh._angle = scfg.phase;
            satellites.push(mesh);
            scene.add(mesh);
        });

        /* ── Rockets ── */
        _tmpV  = new THREE.Vector3();
        _tmpV2 = new THREE.Vector3();
        _up    = new THREE.Vector3(0, 1, 0);
        ROCKET_CONFIGS.forEach(rcfg => {
            const mesh = buildRocket();
            mesh.scale.setScalar(0.50);
            mesh._rcfg  = rcfg;
            mesh._angle = rcfg.phase;
            rocketObjects.push(mesh);
            scene.add(mesh);
        });
    }

    /* ━━━ ANIMATE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ */

    // _up and _tmpV are initialized inside init()

    function animate() {
        frameId = requestAnimationFrame(animate);
        const t = (performance.now() - startTime) / 1000;

        // Cinematic camera drift — stays angled, not top-down
        camera.position.x =  0 + Math.sin(t * 0.035) * 2.0;
        camera.position.y = 10 + Math.cos(t * 0.028) * 1.0;
        camera.lookAt(0, -5, 0);

        starField.rotation.y += 0.00008;

        // Planet orbits + self-spin
        planetGroups.forEach(p => {
            p.pivot.rotation.y += p.cfg.speed * 0.005;
            p.sphere.rotation.y += 0.006;
        });

        // Moon orbits Earth
        if (moonData) moonData.pivot.rotation.y += 0.025;

        // ── Satellites ──────────────────────────────────────────────
        solarPivot.getWorldPosition(_tmpV);
        satellites.forEach(s => {
            const scfg = s._scfg;
            s._angle += scfg.speed * 0.004;
            const a = s._angle, inc = scfg.inc;

            let cx, cy, cz;
            if (scfg.type === 'planet') {
                planetGroups[scfg.planetIdx].meshGroup.getWorldPosition(_tmpV2);
                cx = _tmpV2.x; cy = _tmpV2.y; cz = _tmpV2.z;
            } else {
                cx = _tmpV.x; cy = _tmpV.y; cz = _tmpV.z; // solar centre
            }

            const R = scfg.orbitR * (scfg.type === 'solar' ? 1 : planetGroups[scfg.planetIdx]?.cfg.radius * 3.0 || 1);
            s.position.set(
                cx + Math.cos(a) * R,
                cy + Math.sin(a) * Math.sin(inc) * R,
                cz + Math.sin(a) * Math.cos(inc) * R
            );
            // Slow roll for realism
            s.rotation.y += 0.01;
        });

        // ── Rockets ─────────────────────────────────────────────────
        rocketObjects.forEach(r => {
            const rcfg = r._rcfg;
            r._angle += rcfg.speed * 0.003;
            const a = r._angle, inc = rcfg.inc;

            let cx, cy, cz, R;
            if (rcfg.type === 'solar') {
                // Sweeps around the whole solar system
                cx = _tmpV.x; cy = _tmpV.y; cz = _tmpV.z;
                R  = rcfg.orbitR;
            } else {
                // Near-Earth: elliptical approach orbit
                planetGroups[2].meshGroup.getWorldPosition(_tmpV2); // Earth
                cx = _tmpV2.x; cy = _tmpV2.y; cz = _tmpV2.z;
                // Slightly elliptical: varies between orbitR and orbitR*1.6
                const ecc = 1.0 + 0.6 * Math.sin(a * 0.5);
                R = rcfg.orbitR * planetGroups[2].cfg.radius * 3.2 * ecc;
            }

            r.position.set(
                cx + Math.cos(a) * R,
                cy + Math.sin(a) * Math.sin(inc) * R,
                cz + Math.sin(a) * Math.cos(inc) * R
            );

            const tang = new THREE.Vector3(-Math.sin(a), Math.cos(a)*Math.sin(inc), Math.cos(a)*Math.cos(inc)).normalize();
            r.quaternion.copy(new THREE.Quaternion().setFromUnitVectors(_up, tang));

            const pulse = 3.0 + Math.sin(t * 14 + rcfg.phase) * 1.5;
            if (r._egMat) r._egMat.emissiveIntensity = pulse;
            if (r._el)   r._el.intensity = pulse * 0.9;
        });

        renderer.render(scene, camera);
    }

    function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
    }
</script>

<div
    bind:this={container}
    class="fixed inset-0 -z-10 pointer-events-none"
    style="background: radial-gradient(ellipse at 18% 45%, #07001a 0%, #030010 55%, #000000 100%);"
></div>

<style>
    div { width: 100vw; height: 100vh; }
</style>
