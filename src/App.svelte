<script>
    import { Router, Route } from 'svelte-routing';
    import Home from './Home.svelte';
    import Navbar from './Navbar.svelte';
    import Loading from './Loading.svelte';
    import Background from './Background.svelte';
    import Cursor from './Cursor.svelte';

    let isLoading = true;

    setTimeout(() => {
        isLoading = false;
    }, 5000); // Duration of loading screen (5 seconds)
</script>
<Cursor>
{#if isLoading}
    <Loading />
{:else}
    <main>
        <Background />
        <Navbar />
        <Router>
            <Route path="/" component={Home} />
        </Router>
    </main>
{/if}

<style>
    main {
        position: relative;
        z-index: 1; /* Ensure the main content is above the background */
    }
    *{
		cursor: none !important;
	}
</style>
</Cursor>