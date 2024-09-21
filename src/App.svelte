<script>
    import { Router, Route } from 'svelte-routing';
    import Home from './Home.svelte';
    import Navbar from './Navbar.svelte';
    import Loading from './Loading.svelte';
    import Background from './Background.svelte';
    import Cursor from './Cursor.svelte';
    import Research from './Research.svelte';
    import Tracker from './Tracker.svelte';

    let isLoading = true;

    // Simulate a loading screen for 5 seconds
    setTimeout(() => {
        isLoading = false;
    }, 3000); 
</script>

<!-- Render the loading screen or main content based on isLoading -->
{#if isLoading}
    <div class="loading-container">
        <Loading />
    </div>
{:else}
    <main>
        <Background />
        <Navbar />
        <Router>
            <!-- Corrected the paths and removed ".svelte" from the route paths -->
            <Route path="/" component={Home} />
            <Route path="/research" component={Research} />
            <Route path="/tracker" component={Tracker} />
        </Router>
    </main>
{/if}

<!-- Always render the custom cursor on top -->
<Cursor />

<style>
    main {
        position: relative;
        z-index: 1; /* Ensure the main content is above the background */
    }

    .loading-container {
        position: relative;
        z-index: 2; /* Ensure loading screen is above the background */
    }

    /* Hide the default cursor globally */
    * {
        cursor: none !important;
    }

    /* Ensure the custom cursor has the highest z-index */
    :global(.cursor-star, .trail) {
        z-index: 10000; /* Make sure the custom cursor is above all content */
    }
</style>
