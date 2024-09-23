<script>
  import { onMount } from 'svelte';

  let isClicking = false;
  let isMobile = false;
  let isHoveringClickable = false;

  const updateCursor = (e) => {
    const cursor = document.querySelector('.custom-cursor');
    cursor.style.left = `${e.clientX}px`;
    cursor.style.top = `${e.clientY}px`;
  };

  const handleMouseDown = () => {
    isClicking = true;
    setTimeout(() => {
      isClicking = false;
    }, 200); // Control the time for how long the zoom effect lasts
  };

  const handleTouchStart = () => {
    isMobile = true;
    const cursor = document.querySelector('.custom-cursor');
    cursor.style.display = 'none';
  };

  const handleMouseEnter = (e) => {
    if (e.target.tagName === 'A' || e.target.tagName === 'BUTTON' || e.target.classList.contains('clickable')) {
      isHoveringClickable = true;
    }
  };

  const handleMouseLeave = () => {
    isHoveringClickable = false;
  };

  onMount(() => {
    document.addEventListener('mousemove', updateCursor);
    document.addEventListener('mousedown', handleMouseDown);
    document.addEventListener('touchstart', handleTouchStart);
    document.addEventListener('mouseover', handleMouseEnter);
    document.addEventListener('mouseout', handleMouseLeave);

    return () => {
      document.removeEventListener('mousemove', updateCursor);
      document.removeEventListener('mousedown', handleMouseDown);
      document.removeEventListener('touchstart', handleTouchStart);
      document.removeEventListener('mouseover', handleMouseEnter);
      document.removeEventListener('mouseout', handleMouseLeave);
    };
  });
</script>

<style>
  .custom-cursor {
    position: fixed;
    width: 32px;
    height: 32px;
    background: url('/mouse1.png') no-repeat center center;
    background-size: contain;
    pointer-events: none;
    transform: translate(-50%, -50%);
    z-index: 9999;
    transition: transform 0.2s ease;
  }

  .clicking {
    transform: translate(-50%, -50%) scale(1.3); /* Slight zoom in when clicking */
  }

  .hovering-clickable {
    background: url('/mouse2.png') no-repeat center center; /* New image for clickable elements */
    background-size: contain;
  }

  .hide-default-cursor {
    cursor: none !important;
  }
</style>

<div class="hide-default-cursor">
  <div class="custom-cursor {isClicking ? 'clicking' : ''} {isHoveringClickable ? 'hovering-clickable' : ''}"></div>
  <slot></slot>
</div>
