<script>
	import Contact from './../lib/components/Contact.svelte';
    import "../app.css";
    import Header from '$lib/components/Header.svelte'; 
    import About from '$lib/components/About.svelte';
    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href="/favicon/favicon.ico" />
    <link rel="icon" type="image/png" href="/favicon/favicon-32x32.png" sizes="32x32" />
    <link rel="icon" type="image/png" href="/favicon/favicon-16x16.png" sizes="16x16" />
</svelte:head>

<Header />
<About/>
<Contact />

<main class="flex flex-col min-h-screen">
    <div class="grow">
        {@render children?.()}
    </div>
</main>
