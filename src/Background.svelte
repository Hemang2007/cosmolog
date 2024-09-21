<script>
    import { onMount, onDestroy } from 'svelte';

    let stars = [];
    let mouse = { x: 0, y: 0 };
    const numStars = 100;

    // Function to generate random stars
    function generateStars() {
        stars = [];
        for (let i = 0; i < numStars; i++) {
            stars.push({
                x: Math.random() * window.innerWidth,
                y: Math.random() * window.innerHeight,
                size: Math.random() * 2 + 1,
                offsetX: (Math.random() - 0.5) * 10,
                offsetY: (Math.random() - 0.5) * 10
            });
        }
    }

    // Handle mouse movement
    function handleMouseMove(event) {
        mouse.x = event.clientX;
        mouse.y = event.clientY;
    }

    // Animate stars to follow the mouse
    function animate() {
        stars.forEach(star => {
            star.x += (mouse.x - star.x) * 0.01;
            star.y += (mouse.y - star.y) * 0.01;
        });
        requestAnimationFrame(animate);
    }

    // Handle window resize
    function handleResize() {
        generateStars(); // Regenerate stars on resize to match new window size
    }

    onMount(() => {
        generateStars();
        animate();
        window.addEventListener('resize', handleResize); // Add resize listener
    });

    onDestroy(() => {
        window.removeEventListener('resize', handleResize); // Clean up on destroy
    });
</script>

<style>
    :global(body) {
        margin: 0;
        padding: 0;
        overflow: hidden;
        height: 100vh; /* Full viewport height */
    }

    .background {
        position: fixed; /* Ensure it covers the entire viewport */
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1; /* Ensure it's behind other content */
        pointer-events: none; /* Allow interactions with content above */
        background-color: black;
    }

    .star {
        position: absolute;
        border-radius: 50%;
        background: white;
        box-shadow: 0 0 5px white; /* Glowing effect */
    }
</style>

<!-- Background container with mouse movement handler -->
<div class="background" on:mousemove={handleMouseMove}>
    {#each stars as star}
        <div class="star" style={`top: ${star.y}px; left: ${star.x}px; width: ${star.size}px; height: ${star.size}px;`}></div>
    {/each}
</div>
