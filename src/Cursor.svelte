<script>
  import { onMount } from "svelte";

  let cursorX = 0, cursorY = 0;
  let isClickable = false;
  let isClicked = false; // Track if the mouse is clicked

  // Track mouse movement and update cursor position
  const handleMouseMove = (e) => {
    cursorX = e.clientX;
    cursorY = e.clientY;

    // Check if the hovered element is clickable
    const hoveredElement = document.elementFromPoint(e.clientX, e.clientY);
    if (hoveredElement && (hoveredElement.tagName === 'A' || hoveredElement.tagName === 'BUTTON')) {
      isClickable = true; // Change cursor to clickable style
    } else {
      isClickable = false; // Use default style
    }
  };

  // Handle mouse click to trigger animation
  const handleClick = () => {
    isClicked = true;
    setTimeout(() => {
      isClicked = false; // Reset click after animation
    }, 300); // Duration of animation in ms
  };

  // Set up event listeners
  onMount(() => {
    window.addEventListener("mousemove", handleMouseMove);
    window.addEventListener("click", handleClick);
  });
</script>

<style>
  /* Hide default cursor */
  * {
    cursor: none;
  }

  /* Cursor wrapper with enough space for glow */
  .cursor-wrapper {
    position: absolute;
    width: 80px;
    height: 80px;
    pointer-events: none;
    transform: translate(-50%, -50%);
    z-index: 1000;
  }

  /* Glow effect */
  .glow {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 70px;
    height: 70px;
    background: radial-gradient(circle, rgba(255, 255, 255, 0.5) 0%, rgba(255, 255, 255, 0) 70%);
    transform: translate(-50%, -50%);
    pointer-events: none;
    opacity: 0.9;
  }

  /* Meteorite cursor */
  .custom-cursor,
  .clickable-cursor {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 40px;
    height: 40px;
    background-size: contain;
    background-repeat: no-repeat;
    transform: translate(-50%, -50%);
    pointer-events: none;
    transition: transform 0.1s ease; /* Default transition */
  }

  /* Cursor animation on click */
  .clicked {
    animation: click-animation 0.3s ease forwards;
  }

  /* Clickable cursor (when hovering over links or buttons) */
  .clickable-cursor {
    background-image: url('/mouse2.png');
  }

  .custom-cursor {
    background-image: url('/mouse1.png');
  }

  /* Animation for cursor click effect */
  @keyframes click-animation {
    0% {
      transform: translate(-50%, -50%) scale(1);
    }
    50% {
      transform: translate(-50%, -50%) scale(1.2);
    }
    100% {
      transform: translate(-50%, -50%) scale(1);
    }
  }

  @media (max-width: 768px) {
    .cursor-wrapper {
      display: none;
    }
  }
</style>

<!-- Conditionally render the cursor based on whether the element is clickable and add click animation -->
{#if isClickable}
  <div class="cursor-wrapper" style="left: {cursorX}px; top: {cursorY}px;">
    <div class="glow"></div>
    <div class="clickable-cursor {isClicked ? 'clicked' : ''}"></div>
  </div>
{:else}
  <div class="cursor-wrapper" style="left: {cursorX}px; top: {cursorY}px;">
    <div class="glow"></div>
    <div class="custom-cursor {isClicked ? 'clicked' : ''}"></div>
  </div>
{/if}
