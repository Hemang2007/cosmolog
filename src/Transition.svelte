<script>
    import { fade } from 'svelte/transition';

    export let duration = 500;
    export let isVisible = true; // New prop to control visibility

    const fadeTransition = (node, { duration }) => {
        return {
            duration,
            css: t => `opacity: ${t};`
        };
    };
</script>

{#if isVisible}
    <div transition:fadeTransition={{ duration }}>
        <slot />
    </div>
{/if}
