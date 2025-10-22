<h1 class="underline">Welcome to SvelteKit</h1>
<p class="underline font-bold ">Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<div class="bg-white w-100 h-100" bind:this={c}>
</div>

<style lang="postcss">
@reference "tailwindcss";

:global(html) {
	background-color: theme(--color-gray-500);
}

</style>

<script>
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	let c;

	onMount(()=>{
		gsap.to(c, {x:200, rotation:240, duration:2})
	});
</script>
