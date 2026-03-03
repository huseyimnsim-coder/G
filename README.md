<!DOCTYPE html>
<html lang="az">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Həqiqi AR - Yaxınlaş və Bax</title>
    <style>
        body { margin: 0; overflow: hidden; background: #000; }
        #start-ui {
            position: absolute; width: 100%; height: 100%;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            background: rgba(0,0,0,0.9); z-index: 10; color: white; font-family: sans-serif;
        }
        button { padding: 20px 40px; font-size: 20px; cursor: pointer; border-radius: 10px; border: none; background: #00ff88; font-weight: bold; }
    </style>
</head>
<body>

    <div id="start-ui">
        <h2>Həqiqi Yaxınlaşma Effekti (AR)</h2>
        <p>Telefonu irəli-geri hərəkət etdirərək kuba baxın.</p>
        <button id="ar-button">AR-I BAŞLAT</button>
    </div>

    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
                "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
            }
        }
    </script>

    <script type="module">
        import * as THREE from 'three';
        import { ARButton } from 'three/addons/webxr/ARButton.js';

        let camera, scene, renderer, cube;

        async function init() {
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.01, 20);

            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setPixelRatio(window.devicePixelRatio);
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.xr.enabled = true;
            document.body.appendChild(renderer.domElement);

            // İçi boş yaşıl kub
            const geometry = new THREE.BoxGeometry(0.3, 0.3, 0.3); // 30 sm ölçü
            const edges = new THREE.EdgesGeometry(geometry);
            const material = new THREE.LineBasicMaterial({ color: 0x00ff88, linewidth: 2 });
            cube = new THREE.LineSegments(edges, material);
            
            // Kubu başlanğıcda telefonun 50sm önündə asılı saxlayırıq
            cube.position.set(0, 0, -0.5);
            scene.add(cube);

            const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
            scene.add(light);

            const arBtn = ARButton.createButton(renderer, { requiredFeatures: ['local-floor'] });
            document.body.appendChild(arBtn);
            
            document.getElementById('ar-button').addEventListener('click', () => {
                document.getElementById('start-ui').style.display = 'none';
            });

            renderer.setAnimationLoop(render);
        }

        function render() {
            renderer.render(scene, camera);
        }

        init();
    </script>
</body>
</html>
