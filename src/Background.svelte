<script>
    import { onMount } from 'svelte';

    let stars = [];
    let mouse = { x: 0, y: 0 };

    const numStars = 100;

    // Generate random stars
    for (let i = 0; i < numStars; i++) {
        stars.push({
            x: Math.random() * window.innerWidth,
            y: Math.random() * window.innerHeight,
            size: Math.random() * 2 + 1,
            offsetX: (Math.random() - 0.5) * 10, // Add random offset for glowing effect
            offsetY: (Math.random() - 0.5) * 10
        });
    }

    function handleMouseMove(event) {
        mouse.x = event.clientX;
        mouse.y = event.clientY;
    }

    // Animation loop
    function animate() {
        stars.forEach(star => {
            // Update star position to follow mouse
            star.x += (mouse.x - star.x) * 0.01;
            star.y += (mouse.y - star.y) * 0.01;
        });

        requestAnimationFrame(animate);
    }

    onMount(() => {
        animate();
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

<div class="background" on:mousemove={handleMouseMove}>
    {#each stars as star}
        <div class="star" style={`top: ${star.y}px; left: ${star.x}px; width: ${star.size}px; height: ${star.size}px;`}></div>
    {/each}
</div>
