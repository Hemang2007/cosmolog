<script>
    import { onMount } from "svelte";
    let cursorX = 0, cursorY = 0;
    let starX = 0, starY = 0;
    let trailElements = [];
  
    // Track mouse movement and update star position immediately
    const handleMouseMove = (e) => {
      cursorX = e.clientX;
      cursorY = e.clientY;
    };
  
    onMount(() => {
      window.addEventListener("mousemove", handleMouseMove);
  
      // Create trail elements for the glowing tail effect
      for (let i = 0; i < 10; i++) {
        trailElements.push({ x: 0, y: 0, opacity: i / 10 });
      }
  
      const updateTrail = () => {
        // Update trail elements to follow the star's position
        for (let i = trailElements.length - 1; i > 0; i--) {
          trailElements[i].x = trailElements[i - 1].x;
          trailElements[i].y = trailElements[i - 1].y;
        }
  
        trailElements[0].x = starX;
        trailElements[0].y = starY;
  
        // Make the star instantly move to the cursor position
        starX = cursorX;
        starY = cursorY;
      };
  
      const animate = () => {
        updateTrail();
        requestAnimationFrame(animate);
      };
  
      animate();
    });
  </script>
  
  <style>
    * {
    cursor: none;
}

/* Star core with white color and glowing effect */
.star {
  position: absolute;
  top: 0;
  left: 0;
  width: 2px; /* Slightly larger for a more noticeable glow */
  height: 2px;
  background: radial-gradient(circle, #ffffff 0%, #e0e0e0 100%); /* Smooth white gradient */
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.8); /* Larger white glow */
  transform: translate(-50%, -50%);
  animation: pulse 0.8s infinite alternate; /* Slower, more pronounced pulse */
}

/* Trail follows the star */
.trail {
  position: absolute;
  top: 0;
  left: 0;
  width: 10px; /* Slightly larger for better visibility */
  height: 10px;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.4) 0%, rgba(255, 255, 255, 0) 100%); /* Smooth white gradient */
  border-radius: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.6); /* Softer white glow */
  opacity: 0.4; /* Slightly higher opacity for a more visible trail */
}

/* Pulsating or breathing effect */
@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 15px rgba(255, 255, 255, 0.8); /* Intense glow */
  }
  100% {
    transform: scale(1.2);
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.6); /* Softer glow */
  }
}

  </style>
  
  <!-- Star core attached to the mouse cursor -->
  <div class="star" style="transform: translate({starX}px, {starY}px);"></div>
  
  <!-- Trail follows the star position -->
  {#each trailElements as trail (trail.opacity)}
    <div
      class="trail"
      style="transform: translate({trail.x}px, {trail.y}px); opacity: {trail.opacity};"></div>
  {/each}
  