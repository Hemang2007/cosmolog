<script>
  import { onMount } from "svelte";
  import * as THREE from "three";

  let scene, camera, renderer, earth, starField, junkField;
  let container; // Div container for Three.js renderer

  onMount(() => {
    // Set up the scene
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    renderer = new THREE.WebGLRenderer({ alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);

    // Create Earth sphere
    const earthGeometry = new THREE.SphereGeometry(5, 32, 32);
    const earthTexture = new THREE.TextureLoader().load("/textures/earth_day.jpg");
    const earthMaterial = new THREE.MeshBasicMaterial({ map: earthTexture });
    earth = new THREE.Mesh(earthGeometry, earthMaterial);
    scene.add(earth);

    camera.position.z = 20;

    // Create stars in the background
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

    // Create space junk in orbit around Earth
    const junkCount = 10000; // Number of junk particles
    const junkGeometry = new THREE.BufferGeometry();
    const junkMaterial = new THREE.PointsMaterial({ color: 0xff0000, size: 0.05 }); // Red dots

    const junkVertices = new Float32Array(junkCount * 3);
    const radius = 7; // Distance of the junk from Earth's center

    for (let i = 0; i < junkCount; i++) {
      const theta = Math.random() * Math.PI * 2; // Random angle for horizontal orbit
      const phi = Math.acos((Math.random() * 2) - 1); // Random angle for vertical orbit
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
      earth.rotation.y += 0.001; // Rotate Earth
      starField.rotation.y += 0.0005; // Rotate stars for dynamic background
      junkField.rotation.y += 0.001; // Rotate junk around Earth
      renderer.render(scene, camera);
    };
    animate();

    // Resize handling
    window.addEventListener('resize', onWindowResize, false);
  });

  // Adjust renderer and camera on window resize
  function onWindowResize() {
    const aspectRatio = window.innerWidth / window.innerHeight;
    
    // Adjust FOV based on aspect ratio
    camera.fov = aspectRatio < 1 ? 75 + (25 * (1 - aspectRatio)) : 75; // Increase FOV for narrow screens
    camera.aspect = aspectRatio;
    
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
    background-color: black;
  }
</style>
