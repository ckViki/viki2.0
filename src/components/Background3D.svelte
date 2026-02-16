<script>
    // @ts-nocheck
    import { onMount, onDestroy } from 'svelte';
    import { browser } from '$app/environment';

    let container;
    let scene, camera, renderer, particles;
    let frameId;
    let THREE;

    onMount(async () => {
        if (browser) {
            const threeModule = await import('three');
            THREE = threeModule;
            init();
            animate();
            window.addEventListener('resize', onWindowResize);
        }
    });

    onDestroy(() => {
        if (browser) {
            if (frameId) cancelAnimationFrame(frameId);
            window.removeEventListener('resize', onWindowResize);
            if (renderer) renderer.dispose();
        }
    });

    function init() {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        container.appendChild(renderer.domElement);

        const geometry = new THREE.BufferGeometry();
        const vertices = [];
        for (let i = 0; i < 5000; i++) {
            vertices.push(
                THREE.MathUtils.randFloatSpread(20),
                THREE.MathUtils.randFloatSpread(20),
                THREE.MathUtils.randFloatSpread(20)
            );
        }
        geometry.setAttribute('position', new THREE.Float32BufferAttribute(vertices, 3));

        const material = new THREE.PointsMaterial({
            color: 0xffffff,
            size: 0.02,
            transparent: true,
            opacity: 0.5
        });

        particles = new THREE.Points(geometry, material);
        scene.add(particles);
    }

    function animate() {
        frameId = requestAnimationFrame(animate);
        particles.rotation.x += 0.0005;
        particles.rotation.y += 0.0005;
        renderer.render(scene, camera);
    }

    function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
    }
</script>

<div bind:this={container} class="fixed inset-0 -z-10 bg-black pointer-events-none"></div>

<style>
    div {
        width: 100vw;
        height: 100vh;
    }
</style>
