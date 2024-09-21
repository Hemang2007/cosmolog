<script>
  import { onMount, onDestroy } from "svelte";
  import * as THREE from "three";

  let scene, camera, renderer, earth, starField, junkField;
  let container;

  // Check if it's mobile based on screen width
  let isMobile = window.innerWidth < 768;

  onMount(() => {
    // Initialize the scene
    scene = new THREE.Scene();
    
    // Set camera perspective, adjusting the field of view for mobile
    camera = new THREE.PerspectiveCamera(isMobile ? 100 : 75, window.innerWidth / window.innerHeight, 0.1, 1000);
    renderer = new THREE.WebGLRenderer({ alpha: true });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);

    // Earth setup
    const earthGeometry = new THREE.SphereGeometry(5, 32, 32);
    const earthTexture = new THREE.TextureLoader().load("/textures/earth_day.jpg");
    const earthMaterial = new THREE.MeshBasicMaterial({ map: earthTexture });
    earth = new THREE.Mesh(earthGeometry, earthMaterial);
    scene.add(earth);

    // Adjust the camera distance for mobile
    camera.position.z = isMobile ? 35 : 20;

    // Starfield setup
    const starGeometry = new THREE.BufferGeometry();
    const starMaterial = new THREE.PointsMaterial({ color: 0xffffff, size: 0.7 });

    const starVertices = [];
    for (let i = 0; i < 10000; i++) {
      const x = THREE.MathUtils.randFloatSpread(2000);
      const y = THREE.MathUtils.randFloatSpread(2000);
      const z = THREE.MathUtils.randFloatSpread(2000);
      starVertices.push(x, y, z);
    }

    starGeometry.setAttribute('position', new THREE.Float32BufferAttribute(starVertices, 3));
    starField = new THREE.Points(starGeometry, starMaterial);
    scene.add(starField);

    // Space junk setup
    const junkCount = 10000;
    const junkGeometry = new THREE.BufferGeometry();
    const junkMaterial = new THREE.PointsMaterial({ color: 0xff0000, size: isMobile ? 0.02 : 0.05 });

    const junkVertices = new Float32Array(junkCount * 3);
    const radius = 7;

    for (let i = 0; i < junkCount; i++) {
      const theta = Math.random() * Math.PI * 2;
      const phi = Math.acos((Math.random() * 2) - 1);
      const x = radius * Math.sin(phi) * Math.cos(theta);
      const y = radius * Math.sin(phi) * Math.sin(theta);
      const z = radius * Math.cos(phi);

      junkVertices[i * 3] = x;
      junkVertices[i * 3 + 1] = y;
      junkVertices[i * 3 + 2] = z;
    }

    junkGeometry.setAttribute('position', new THREE.BufferAttribute(junkVertices, 3));
    junkField = new THREE.Points(junkGeometry, junkMaterial);
    scene.add(junkField);

    // Animation loop
    const animate = function () {
      requestAnimationFrame(animate);
      earth.rotation.y += 0.001;
      starField.rotation.y += 0.0005;
      junkField.rotation.y += 0.001;
      renderer.render(scene, camera);
    };
    animate();

    // Handle resizing
    window.addEventListener('resize', onWindowResize);

    onDestroy(() => {
      window.removeEventListener('resize', onWindowResize);
    });
  });

  function onWindowResize() {
    const isMobile = window.innerWidth < 768;
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.fov = isMobile ? 100 : 75; // Increase FOV for mobile
    camera.position.z = isMobile ? 35 : 20; // Adjust camera distance for mobile
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
</script>

<!-- Container for Three.js scene -->
<div bind:this={container} class="three-container"></div>

<style>
  .three-container {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    position: absolute;
    top: 0;
    left: 0;
    background-color: black;
  }
</style>
