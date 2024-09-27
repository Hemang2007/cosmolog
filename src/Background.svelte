<script>
    import * as THREE from 'three';
    import { onMount } from 'svelte';
  
    let scene, camera, renderer, stars = [];
    let canvas; // Reference to the canvas
  
    const initStars = () => {
      const starGeometry = new THREE.SphereGeometry(0.2, 24, 24);
      const starMaterial = new THREE.MeshBasicMaterial({ color: 0xffffff });
  
      for (let i = 0; i < 50000; i++) {
        const star = new THREE.Mesh(starGeometry, starMaterial);
        star.position.x = (Math.random() - 0.5) * 2000;
        star.position.y = (Math.random() - 0.5) * 2000;
        star.position.z = Math.random() * 2000;
        stars.push(star);
        scene.add(star);
      }
    };
  
    const animateStars = () => {
      stars.forEach(star => {
        star.position.z -= 5;
        if (star.position.z < -500) {
          star.position.z = 1000;
        }
      });
    };
  
    onMount(() => {
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 2000);
      camera.position.z = 5;
  
      // Initialize renderer with canvas element
      renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
  
      initStars();
  
      const animate = () => {
        requestAnimationFrame(animate);
        animateStars();
        renderer.render(scene, camera);
      };
  
      animate();
  
      window.addEventListener('resize', () => {
        renderer.setSize(window.innerWidth, window.innerHeight);
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
      });
  
      return () => {
        window.removeEventListener('resize', () => {});
      };
    });
  </script>
  
  <canvas bind:this={canvas}></canvas>
  
  <style>
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      display: block;
      z-index: -1;
      width: 100vw;
      height: 100vh;
    }
  </style>
  