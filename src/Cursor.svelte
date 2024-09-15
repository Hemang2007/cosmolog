<script>
  import { onMount } from "svelte";

  let cursorX = 0, cursorY = 0;
  let isClickable = false;

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

  // Set up event listener for mouse movement
  onMount(() => {
    window.addEventListener("mousemove", handleMouseMove);
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
    width: 80px; /* Larger size for the wrapper to contain the glow */
    height: 80px;
    pointer-events: none; /* Allow clicking through */
    transform: translate(-50%, -50%);
    z-index: 1000; /* High z-index to ensure it stays above other elements */
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
  .custom-cursor {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 40px; /* Cursor stays 40px */
    height: 40px;
    background-size: contain;
    background-repeat: no-repeat;
    transform: translate(-50%, -50%);
    pointer-events: none;
  }

  /* Clickable cursor (when hovering over links or buttons) */
  .clickable-cursor {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 40px; /* Cursor stays 40px */
    height: 40px;
    background-size: contain;
    background-repeat: no-repeat;
    transform: translate(-50%, -50%);
    pointer-events: none;
  }

</style>

<!-- Conditionally render the cursor based on whether the element is clickable -->
{#if isClickable}
  <div class="cursor-wrapper" style="left: {cursorX}px; top: {cursorY}px;">
    <div class="glow"></div>
    <div class="clickable-cursor" style="background-image: url('/mouse2.png');"></div>
  </div>
{:else}
  <div class="cursor-wrapper" style="left: {cursorX}px; top: {cursorY}px;">
    <div class="glow"></div>
    <div class="custom-cursor" style="background-image: url('/mouse1.png');"></div>
  </div>
{/if}
