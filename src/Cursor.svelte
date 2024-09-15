<script>
    let mouseX = 0;
    let mouseY = 0;
    let starTrail = [];
  
    // Track the mouse position
    const handleMouseMove = (event) => {
      mouseX = event.clientX;
      mouseY = event.clientY;
  
      // Add the current mouse position to the trail
      starTrail = [{ x: mouseX, y: mouseY }, ...starTrail].slice(0, 15);
    };
  
    // Add a delay effect for the shooting star's tail
    const getTrailStyle = (index) => {
      return `transform: translate(${starTrail[index]?.x}px, ${starTrail[index]?.y}px); opacity: ${1 - index * 0.1};`;
    };
  </script>
  
  <style>
    .cursor-star {
      position: fixed;
      top: 0;
      left: 0;
      width: 10px;
      height: 10px;
      background: radial-gradient(circle, white, rgba(255,255,255,0));
      border-radius: 50%;
      pointer-events: none;
      transition: transform 0.02s ease;
    }
  
    .trail {
      position: fixed;
      width: 6px;
      height: 6px;
      background-color: white;
      border-radius: 50%;
      pointer-events: none;
    }
  </style>
  
  <div on:mousemove={handleMouseMove} style="width: 100vw; height: 100vh;">
    <!-- Main shooting star cursor -->
    <div
      class="cursor-star"
      style="transform: translate({mouseX}px, {mouseY}px);"
    ></div>
  
    <!-- Trail effect -->
    {#each starTrail as trail, index (index)}
      <div
        class="trail"
        style={getTrailStyle(index)}
      ></div>
    {/each}
  </div>
  