<script>
  import { onMount } from "svelte";
  import * as THREE from "three";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

  let scene, camera, renderer, earth, starField, junkField;
  let container;
  let junkSpeed = 0.0001;
  const junkCount = 50000;
  const minRadius = 5.12;
  const maxRadius = 6.75;
  let controls;
  let isTextureLoaded = false;

  // Arrays to store theta, phi angles and radii
  let thetaArray = [];
  let phiArray = [];
  let radiusArray = [];

  // Loading manager for progress handling
  const loadingManager = new THREE.LoadingManager();
  loadingManager.onProgress = function (url, itemsLoaded, itemsTotal) {
    console.log(`Loading file: ${url}. Loaded ${itemsLoaded} of ${itemsTotal} files.`);
  };
  loadingManager.onLoad = function () {
    isTextureLoaded = true; // Set texture loaded flag
  };

  // Asynchronous texture loader
  const textureLoader = new THREE.TextureLoader(loadingManager);

  onMount(() => {
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    renderer = new THREE.WebGLRenderer({ alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);

    // Create Earth sphere with placeholder texture
    const earthGeometry = new THREE.SphereGeometry(5, 32, 32);
    const placeholderTexture = textureLoader.load("/textures/earth_lowres.png"); // Low-res initial texture
    const earthMaterial = new THREE.MeshBasicMaterial({ map: placeholderTexture });
    earth = new THREE.Mesh(earthGeometry, earthMaterial);
    scene.add(earth);

    camera.position.z = 20;

    // Load high-resolution Earth texture asynchronously
    textureLoader.load("/textures/earth.png", (highResTexture) => {
      earth.material.map = highResTexture; // Replace the low-res texture
      earth.material.needsUpdate = true; // Ensure material is updated
    });

    // Stars and space junk setup (same as before)
    const starGeometry = new THREE.BufferGeometry();
    const starMaterial = new THREE.PointsMaterial({ color: 0xffffff });

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

    const junkGeometry = new THREE.BufferGeometry();
    const junkMaterial = new THREE.PointsMaterial({ color: 0xff0000, size: 0.05 });
    const junkVertices = new Float32Array(junkCount * 3);

    for (let i = 0; i < junkCount; i++) {
      const theta = Math.random() * Math.PI * 2;
      const phi = Math.acos((Math.random() * 2) - 1);
      const randomRadius = THREE.MathUtils.randFloat(minRadius, maxRadius);

      const x = randomRadius * Math.sin(phi) * Math.cos(theta);
      const y = randomRadius * Math.sin(phi) * Math.sin(theta);
      const z = randomRadius * Math.cos(phi);

      junkVertices[i * 3] = x;
      junkVertices[i * 3 + 1] = y;
      junkVertices[i * 3 + 2] = z;

      thetaArray.push(theta);
      phiArray.push(phi);
      radiusArray.push(randomRadius);
    }

    junkGeometry.setAttribute('position', new THREE.BufferAttribute(junkVertices, 3));
    junkField = new THREE.Points(junkGeometry, junkMaterial);
    scene.add(junkField);

    controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.25;
    controls.enableZoom = true;
    controls.maxDistance = 50;
    controls.minDistance = 10;

    const animate = function () {
      requestAnimationFrame(animate);

      const junkPositions = junkField.geometry.attributes.position.array;

      for (let i = 0; i < junkCount; i++) {
        thetaArray[i] += junkSpeed;

        const randomRadius = radiusArray[i];

        const x = randomRadius * Math.sin(phiArray[i]) * Math.cos(thetaArray[i]);
        const y = randomRadius * Math.sin(phiArray[i]) * Math.sin(thetaArray[i]);
        const z = randomRadius * Math.cos(phiArray[i]);

        junkPositions[i * 3] = x;
        junkPositions[i * 3 + 1] = y;
        junkPositions[i * 3 + 2] = z;
      }

      junkField.geometry.attributes.position.needsUpdate = true;

      controls.update();
      renderer.render(scene, camera);
    };
    animate();

    window.addEventListener('resize', onWindowResize, false);
  });

  function onWindowResize() {
    const aspectRatio = window.innerWidth / window.innerHeight;
    camera.fov = aspectRatio < 1 ? 75 + (25 * (1 - aspectRatio)) : 75;
    camera.aspect = aspectRatio;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
</script>

<!-- Container for Three.js scene -->
<div bind:this={container} class="three-container">
  {#if !isTextureLoaded}
    <div class="loading-overlay">Loading...</div> <!-- Loading screen -->
  {/if}
</div>

<style>
  .three-container {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    background-color: black;
    position: relative;
  }

  .loading-overlay {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 24px;
  }
</style>
