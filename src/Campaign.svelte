<script>
    import { onMount } from 'svelte';
  
    let counters = {
      totalUntraceable: 100_000_000_000_000,
      total1cm: 100_000_000,
      between1cmAnd10cm: 900_000,
      totaltraceable: 36000,
      totalLEO: 170_000_000,
    };
  
    let animatedCounts = {
      totalUntraceable: 0,
      totalLEO: 0,
      total1cm: 0,
      between1cmAnd10cm: 0,
      totaltraceable: 0,
    };

    let animationCompleted = false; // New variable to track animation state

    function animateCounter(key, target) {
      let start = 0;
      const duration = 2000; // 2 seconds
      const stepTime = 10; // Update every 10ms
      const steps = duration / stepTime;
      const increment = target / steps;
  
      const interval = setInterval(() => {
        start += increment;
        if (start >= target) {
          start = target;
          clearInterval(interval);
          animationCompleted = true;
        }
        animatedCounts[key] = Math.floor(start);
      }, stepTime);
    }
  
    onMount(() => {
      Object.keys(counters).forEach(key => animateCounter(key, counters[key]));
    });
  </script>
  
  <style>
    :global(body) {
      color: white; /* Set default text color to white */
    }
    
    .wrapper {
    max-height: 100vh; /* Ensure it takes full height of viewport */
    padding: 2rem; /* Add padding */
  }
    .counter {
      font-size: 4rem; /* Make numbers big */
      margin: 1rem 0;
      text-align: center;
      font-family: Futura;
    }
  
    .tag {
      font-size: 1.5rem; /* Smaller font for tags */
      text-align: center;
      opacity: 0.8; /* Slightly transparent for tags */
      font-family: Libre;
    }

    .plus {
    font-size: 4rem; /* Match font size of counter */
    display: inline; /* Display inline with number */
    margin-left: 0.5rem;
    top: 0;
  }
  </style>

<div class="wrapper">
  <div>
    <div class="counter">
      {animatedCounts.totalUntraceable}
      {#if animationCompleted}<span class="plus">+</span>{/if}
      <div class="tag">Total Untraceable Pieces</div>
    </div>
    <div class="counter">
      {animatedCounts.totalLEO}
      {#if animationCompleted}<span class="plus">+</span>{/if}
      <div class="tag">Approx Debirs in LEO</div>
    </div>
    <div class="counter">
      {animatedCounts.total1cm}
      {#if animationCompleted}<span class="plus">+</span>{/if}
      <div class="tag">Less than 1cm in diameter</div>
    </div>
    <div class="counter">
      {animatedCounts.between1cmAnd10cm}
      {#if animationCompleted}<span class="plus">+</span>{/if}
      <div class="tag">Between 1cm and 10cm in diameter</div>
    </div>
    <div class="counter">
      {animatedCounts.totaltraceable}
      {#if animationCompleted}<span class="plus">+</span>{/if}
      <div class="tag">Traceable Objects</div>
    </div>
  </div>
</div>