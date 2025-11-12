<script>
    import { onMount } from 'svelte';
    let MapComponent;
    
    const MAPTILER_API_KEY = "HntKGi9dc2dEqA7Yfllk";
    let loaded = false;

    onMount(async () => {
        const module = await import('./MapContainer.svelte');
        MapComponent = module.default;
        setTimeout(() => {
            loaded = true;
        }, 1000);
    });
</script>

{#if loaded}
    <svelte:component this={MapComponent} {MAPTILER_API_KEY} />
{:else}
    <div class="flex items-center justify-center w-screen h-screen bg-gray-900 text-white">
        <svg class="animate-spin h-5 w-5 mr-3 text-orange-500" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        Loading Map...
    </div>
{/if}
